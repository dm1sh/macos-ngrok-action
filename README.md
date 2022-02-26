# Mac OS Ngrok tunelling Github Action

This is a Github Action that can be used in your Github Workflow to tunnel incoming/outgoing TCP traffic in your workflow environment running on Mac OS.

## How to use

This action accepts the following parameters:

| Name | Description | Required  | Default |
| ------------- |-------------|-----|-----|
| timeout | After this timeout the deployment will automatically shutdown the tunelling and therefore stop the action. (max is 6 hours) | No | 1h |
| protocol | Protocol that will be used by Ngrok: tcp or http | No | tcp |
| port | The port in localhost to forward traffic from/to | Yes | - |
| ngrok_authtoken | Your ngrok authtoken | Yes | - |

You also need to set up `NGROK_AUTHTOKEN` secret with token required from [here](https://dashboard.ngrok.com/get-started/your-authtoken)

Example usage:

```yaml
name: CI
on: push

jobs:

  deploy:
    name: Deploy challenge
    runs-on: macos-latest
    needs: cancel

    steps:
    - name: Checkout
      uses: actions/checkout@v2

    - name: Install docker-compose
      run: brew install docker docker-machine docker-compose
    
    - name: Run container
      run: docker-compose up -d 
    
    - uses: dm1sh/macos-ngrok-action@v0.0.1
      with:
        timeout: 1h
        port: 8080
        ngrok_authtoken: ${{ secrets.NGROK_AUTHTOKEN }}
```
