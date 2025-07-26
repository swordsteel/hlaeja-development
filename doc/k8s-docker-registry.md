# K8s Docker Registry Configuration

**How to make JSON Configuration**

```json=
{
  "auths": {
    "<your-registry>": {
      "username": "<your-username>",
      "password": "<your-password>",
      "email": "<your-email@example.com>",
      "auth": "<base64-of-your-username:your-password>"
    }
  }
}
```

**Replace Values**

- **Replace** <your-registry>: Use the hostname of your Gitea instance (e.g., registry.example.com).
- **Replace** <your-username>: Use your Gitea username (e.g., user1).
- **Replace** <your-password>: Use your Gitea personal access token generated with read:package scope (e.g., abc123).
- **Replace** <your-email>: Use your email address (e.g., user1@example.com).

**Linux Command**

```bash
echo -n 'your-username:your-password' | base64 -w 0
```

witch gives `eW91ci11c2VybmFtZTp5b3VyLXBhc3N3b3Jk` then we use it in the `auth`

```bash
echo -n '{"auths":{"<your-registry>":{"username":"your-username","password":"your-password","email":"your-email","auth":"eW91ci11c2VybmFtZTp5b3VyLXBhc3N3b3Jk"}}}' | base64 -w 0
```

witch give `eyJhdXRocyI6eyI8eW91ci1yZWdpc3RyeT4iOnsidXNlcm5hbWUiOiJ5b3VyLXVzZXJuYW1lIiwicGFzc3dvcmQiOiJ5b3VyLXBhc3N3b3JkIiwiZW1haWwiOiJ5b3VyLWVtYWlsIiwiYXV0aCI6ImVXOTFjaTExYzJWeWJtRnRaVHA1YjNWeUxYQmhjM04zYjNKayJ9fX0=`
