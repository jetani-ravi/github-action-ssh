# github-action-ssh

## Configure SSH into aws ec2

- How to handle SSH keys with EC2-github actions [Learn more](https://zellwk.com/blog/github-actions-deploy/)

### Declare these git secrets
- SSH_PRIVATE_KEY
- HOST_NAME / IP_ADDRESS
- USER_NAME

```yml
name: Deploy

on:
push:
branches: [dev]

jobs:
Deploy:
name: Deploy to EC2
runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Build & Deploy
        env:
          PRIVATE_KEY: ${{ secrets.SSH_PRIVATE_KEY }}
          HOSTNAME: ${{ secrets.SSH_HOST }}
          USER_NAME: ${{ secrets.USER_NAME }}

        run: |
          echo "$PRIVATE_KEY" > private_key && chmod 600 private_key
          ssh -o StrictHostKeyChecking=no -i private_key ${USER_NAME}@${HOSTNAME} '

            # Now we have access to EC2, let's start the deployment.
            cd /home/ubuntu/<PROJECT_DIRECTORY> &&
            git checkout dev &&
            git fetch --all &&
            git reset --hard origin/dev &&
            git pull origin dev &&
            sudo npm ci --production &&
            sudo pm2 restart ./dist/index.js
          '
```
