# Examples

A full Red-Green-Refactor walkthrough, followed by the three common test patterns. Every cycle is one test, one implementation, then the next.

---

## Worked example: an OAuth URL generator

The feature: a `generateOAuthUrl(provider)` function that returns the authorization URL for a given identity provider. We build it one failing test at a time.

### Cycle 1

Red: write the simplest test and watch it fail.

```javascript
test('generateOAuthUrl creates a valid Google URL', () => {
  const url = generateOAuthUrl('google');

  expect(url).toContain('accounts.google.com');
  expect(url).toContain('client_id=');
});

// npm test
// FAIL: generateOAuthUrl is not defined
```

Green: write the minimal code that passes. Hardcoding is fine here; the next test will force generalization.

```javascript
export function generateOAuthUrl(provider) {
  return 'https://accounts.google.com/o/oauth2/v2/auth?client_id=123';
}

// npm test
// PASS
```

### Cycle 2

Red: add support for a second provider. The hardcoded Google URL now fails.

```javascript
test('generateOAuthUrl creates a valid GitHub URL', () => {
  const url = generateOAuthUrl('github');
  expect(url).toContain('github.com');
});

// npm test
// FAIL: expected a GitHub URL, got the Google URL
```

Green: introduce a provider lookup. This is the minimum that satisfies both tests.

```javascript
const PROVIDERS = {
  google: 'https://accounts.google.com/o/oauth2/v2/auth',
  github: 'https://github.com/login/oauth/authorize',
};

export function generateOAuthUrl(provider) {
  const baseUrl = PROVIDERS[provider];
  return `${baseUrl}?client_id=123`;
}

// npm test
// PASS
```

### Cycle 3

Red: pin the error behavior for an unknown provider.

```javascript
test('generateOAuthUrl throws for an unknown provider', () => {
  expect(() => generateOAuthUrl('unknown')).toThrow('Unknown provider');
});

// npm test
// FAIL: did not throw
```

Green: add the guard.

```javascript
export function generateOAuthUrl(provider) {
  const baseUrl = PROVIDERS[provider];

  if (!baseUrl) {
    throw new Error(`Unknown provider: ${provider}`);
  }

  return `${baseUrl}?client_id=123`;
}

// npm test
// PASS
```

### Refactor

All three tests are green, so the code is safe to improve. Pull the client IDs out of the hardcoded string and into configuration, and add types. Behavior does not change, so the tests stay green.

```javascript
const OAUTH_PROVIDERS = {
  google: {
    authUrl: 'https://accounts.google.com/o/oauth2/v2/auth',
    clientId: process.env.GOOGLE_CLIENT_ID,
  },
  github: {
    authUrl: 'https://github.com/login/oauth/authorize',
    clientId: process.env.GITHUB_CLIENT_ID,
  },
};

export function generateOAuthUrl(provider: string): string {
  const config = OAUTH_PROVIDERS[provider];

  if (!config) {
    throw new Error(`Unknown provider: ${provider}`);
  }

  return `${config.authUrl}?client_id=${config.clientId}`;
}

// npm test
// PASS (tests still green after refactor)
```

### Final test suite

```javascript
describe('generateOAuthUrl', () => {
  test('creates a valid Google OAuth URL', () => {
    const url = generateOAuthUrl('google');
    expect(url).toContain('accounts.google.com');
    expect(url).toContain('client_id=');
  });

  test('creates a valid GitHub OAuth URL', () => {
    const url = generateOAuthUrl('github');
    expect(url).toContain('github.com');
    expect(url).toContain('client_id=');
  });

  test('throws for an unknown provider', () => {
    expect(() => generateOAuthUrl('unknown')).toThrow('Unknown provider: unknown');
  });
});

// All tests pass. Every branch arrived with its test.
```

Notice the shape of it: each test failed first, each implementation was the minimum to pass, and the refactor only happened once the tests could prove it was safe. The third test (the error case) is what made the guard clause necessary; the design was pulled forward by the tests, not designed up front.

---

## Common patterns

### Pattern 1: data transformation

A function that maps input to output: formatters, parsers, calculators. Test the happy path, an edge case, and an error case.

```javascript
test('happy path', () => expect(transform(validInput)).toBe(expectedOutput));
test('edge case', () => expect(transform(edgeCase)).toBe(edgeOutput));
test('error case', () => expect(() => transform(invalid)).toThrow());
```

### Pattern 2: state change

A function that mutates state or persists to a database: CRUD operations, state machines. Assert on the observable result of the change, not on the internal call.

```javascript
test('creates a resource', async () => {
  await createResource(data);

  const resource = await db.find(data.id);
  expect(resource).toBeDefined();
});
```

### Pattern 3: side effects

A function that sends an email, calls an external API, or logs. Mock the external dependency and assert on what was sent, not on the internal wiring.

```javascript
test('sends a welcome email', async () => {
  const mockSend = jest.fn();

  await sendWelcomeEmail(user, { sendEmail: mockSend });

  expect(mockSend).toHaveBeenCalledWith(
    expect.objectContaining({ to: user.email, subject: 'Welcome' })
  );
});
```

In all three patterns the assertion is on behavior: the returned value, the persisted record, the message sent. None of them assert on a private method or an internal field. That is what keeps the tests alive across refactors.
