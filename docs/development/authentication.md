# Authentication
- Use API keys, personal access tokens, and other "magic string" credentials sparingly.  Should be used only when the scope of permissions is very small, signatures are rotated, and both systems are trusted.  In all cases, authorization policies must limit access to the minimum set of allowable actions. Examples:
    - Producer sends data to a single endpoint.
    - Consumer of a webhook or periodic poll to a single endpoint.
    - Both systems are trusted and keys are stored in secret storage like Azure Key Vault or Kubernetes Secrets.
    - Other forms of authentication are not supported by both platforms.
- Use an identity platform if possible, fallback to a trusted identity management library, such as identity server, when that is not possible.