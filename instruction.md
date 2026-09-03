The Flask CLI ships `run`, `shell`, and `routes` commands, but there is no way to inspect an application's configuration from the command line. Today the only options are opening `flask shell` or writing code that reads `app.config`. Add a `flask config` command that prints the application's effective configuration.

## Required behavior

1. `flask config` is registered on the default Flask CLI group alongside `run`, `shell`, and `routes`, so `flask --help` lists it. It runs inside an application context and reads `current_app.config`.
2. With no arguments, print the full effective configuration (Flask's default keys such as `DEBUG` and `SECRET_KEY` merged with everything the app set), one entry per line in the form `KEY = VALUE`, where `VALUE` is the Python `repr()` of the stored value. Lines are sorted by key.
3. `flask config KEY` prints only the `repr()` of that key's value. Keys are matched exactly (case-sensitive). If `KEY` is not present in the config, print an error that names the key and exit with a non-zero status.
4. `--json` prints the whole configuration as a single JSON object whose keys are sorted; combined with `KEY` it prints only that value as JSON. Serialize with the application's JSON provider so provider customizations apply. Values the provider cannot serialize (for example `datetime.timedelta`, as in `PERMANENT_SESSION_LIFETIME`) must fall back to their `repr()` string rather than failing. A missing key with `--json` reports the same error and non-zero exit as without it.
5. Output goes to standard output. Values must reflect what is actually stored in `app.config`, so app-set values override defaults.
6. Existing commands, their options, and `flask --help` output for them are unchanged, and the existing test suite must keep passing.

Document the new command in the CLI docs and add a changelog entry.

IMPORTANT: Please work on this in a new branch from main and commit everything when you are done.
