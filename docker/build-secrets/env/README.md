# Lab: Env Variables

```bash
# define variables
export USERNAME=jeffrey_lebowski
export PASSWORD=white_russian

# create builder image for testing purpose
docker build \
  --secret id=username,env=USERNAME \
  --secret id=password,env=PASSWORD \
  -t secret-env-builder \
  --target builder \
  .

# check if poetry has set credentials right
docker run \
  --entrypoint=/bin/sh \
  secret-env-builder \
  -c "cat /root/.config/pypoetry/auth.toml"
```