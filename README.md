
## About

Github Action to get go modules (including private repos, but need github token).

## Configuration

1. Generate Secrets for `CI_TOKEN`
   
    - Click your avatar
   
    - Click "Settings"
   
    - Click "Developer settings"
   
    - Click "Personal access tokens"

    - Click "Generate new token"
    
    - Check `repo` and `read:org`

2. Configure `CI_TOKEN`, `USERNAME` and `GOPRIVATE`
   
    - Go to your repo's page
   
    - Click "Settings"

    - Click "Secrets"

    - Click "New repository secret" to set necessary secrets
    
        - CI_TOKEN: the secrets generated before
        
        - USERNAME: the github username of the user to which `CI_TOKEN` belongs
        
        - GOPRIVATE: need to be set to the value of `GOPRIVATE` (for example: `github.com/ncuhome`)
    

## Example

```yaml
name: my-workflow

on:
  push:
    branches: [ main ]

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:

      - name: Set up Go 1.x
        uses: actions/setup-go@v2
        with:
          go-version: 1.14
        id: go

      - name: Check out repository
        uses: actions/checkout@v2

      - name: Get Go Modules
        uses: kuhsinyv/go-module-action@v1
        env:
          USERNAME: ${{ secrets.USERNAME }}
          CI_TOKEN: ${{ secrets.CI_TOKEN }}
          GOPRIVATE: ${{ secrets.GOPRIVATE }}

      - name: Build
        run: go build
```