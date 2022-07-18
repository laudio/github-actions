# Status Check

Run preliminary status checks in a JavaScript/TypeScript repository

### Inputs Required

###### path

The source directory path where status should be checked.

###### skip-build

Skips build command.

###### token

GITHUB_TOKEN or a repo scoped PATH.

###### sonar_host_url

This is the url where your sonarqube instance is hosted.

###### sonar_token

This is the token created in your sonarqube instance.

###### service_name

This is an optional input for service name in monorepos

### Usage

```
  - name: Checkout
    uses: actions/checkout@v2

  - name: Use Node 16
  uses: actions/setup-node@v2
  with:
    always-auth: true
    node-version: "16"
    registry-url: "https://npm.pkg.github.com"
    cache: "yarn"
    cache-dependency-path: "**/yarn.lock"

- name: Connect VPN
  run: |
    sudo apt-get install openvpn
    echo -e "${{ secrets.VPN_USER }}\n${{ secrets.VPN_PASS }}" > ~/.vpncreds
    sudo openvpn --config config.ovpn --auth-user-pass ~/.vpncreds --daemon
    sleep 10s

  - name: Status Check
    if: contains(fromJson(needs.detect-changed-files.outputs.services), 'files')
    uses: laudio/github-actions/status-check@v1.0.21
    with:
      path: "services/export"
      token: ${{ secrets.LAUDIO_GITHUB_TOKEN }}
      sonar_token: ${{secrets.SONAR_TOKEN}}
      sonar_host_url: ${{secrets.SONAR_HOST_URL}}
      service_name: "export"
```
