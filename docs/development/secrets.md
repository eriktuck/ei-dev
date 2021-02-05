# Secrets

Secrets are any pieces of information you don't want to store directly in your code. Generally, this includes passwords and other credentials for logging into third-party applications. If you need to provide a password to sign into a database, how do you make the password available without storing it in the code itself?

### The secrets/ folder

I add a `secrets/` folder to my projects and save credentials there as `JSON` files. MAKE SURE TO INCLUDE `secrets/` IN YOUR `.gitignore` FILE (but add a [.keep file](../git/additional-reference.md#adding-empty-folders))!

Here's what a `my_secrets_file.json` file would look like with a secret key:

```json
{
    "USERNAME": "<save_key_here>",
    "PASSWORD": "<save_password_here"
}
```

Note that `JSON` files require double quotes.

### Accessing secrets

You can read that into python just like a dictionary:

```python
import json

with open('secrets/my_secrets_file.json') as f:
    my_creds = json.load(f)

USERNAME = my_creds.get("USERNAME")
PASSWORD = my_creds.get("PASSWORD")
```

### Don't commit secrets

When you create the `secrets/` folder, go ahead and add it to your `.gitignore` file, even before you save anything there, so you don't forget. Add a `.keep` file so that collaborators know that there are credentials required for this project. Add these lines to the `.gitignore` file:

```
secrets/*
!secrets/.keep
```

If you want, you can save the structure of the credentials in the `.keep` file to help collaborators set it up correctly (obviously don't actually put the secret info here):

```
- my_secrets_file.json:
{
    "USERNAME": "<save_key_here>",
    "PASSWORD": "<save_password_here"
}

- credentials.json:
...
```

### Remove secrets from Github

You will, sooner or later, commit a secret to GitHub. GitHub (or the third-party app) may even email you right after you did it (e.g., Google will email you if you commit google credentials to GitHub). To remove the file, use `git rm -r --cached <filename>`. Then commit and push the changes to GitHub.

### Auth packages

Some packages such as `google-auth` or `oauthlib` access and use credentials stored as a JSON file.

### Environment Variables

Another way to store secrets is to use [environment variables](environment-variables.md). You will need to use environment variables (rather than `JSON` files) with some platforms like Heroku. Here's how you can structure code to seamlessly transition between local testing and production:

```python
# Local dev
try:
    with open('secrets/passwords.json') as f:
        VALID_USERNAME_PASSWORD_PAIRS = json.load(f)
# Heroku dev
except:
    json_creds = os.environ.get("VALID_USERNAME_PASSWORD_PAIRS")
    VALID_USERNAME_PASSWORD_PAIRS = json.loads(json_creds)    
```

