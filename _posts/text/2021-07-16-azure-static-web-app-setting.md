---

title:  "Azure Static Web App CI/CD 및 Config 설정"
excerpt: "Azure Static Web App CI/CD 및 Config 설정"

categories:
  - 일반
tags:
  - Text
last_modified_at: 2021-07-16

---

## 1. Azure Static Web App Github Actions Config

azure static web app 최우선 config 파일   
내용은 모든 링크를 rewrite 한다.

`src/assets/azureConfig/staticwebapp.config.json`

```
 {
    "routes": [
      {
        "route": "/*",
        "serve": "/index.html",
        "statusCode": 200
      }
    ], 
     "navigationFallback": {
          "rewrite": "/index.html",
          "exclude": ["/images/*.{png,jpg,gif}", "/css/*"]
        }
  }
```

Github Actions의 워크플로우 파일   
내용은 환경 변수 .env 파일 추가 후 yarn으로 빌드하고   
Token의 Azure Static Web App에 Deploy 및 Config 설정

`.github/workflows/deploy.yml`

```
name: CI/CD

on:
  push:
    branches:
    - develop
  pull_request:
    types: [closed]
    branches:
    - develop
    
jobs:
  build_and_deploy_app:
    runs-on: ubuntu-latest
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    name: Build and Deploy Job
    steps:
    - uses: actions/checkout@v2
    
    
    - name: Setting .env
      run: |
        touch .env
        echo "${{ secrets.ENV }}" >> .env
        # 한 줄은 echo "" > .env로 처리하는게 깔끔한가?
    
    - name: Setup Node
      uses: actions/setup-node@v2
      with:
        node-version: '14'
        cache: 'yarn'
        
    - name: Build Vue App
      run: |
        yarn install
        yarn build
    - name: Deploy
      uses: Azure/static-web-apps-deploy@v0.0.1-preview
      with:
        azure_static_web_apps_api_token: '${{secrets.STATIC_APP_TOKEN}}'
        repo_token: ${{ secrets.GITHUB_TOKEN }}
        action: 'upload'
        config_file_location: 'src/assets/azureConfig'
        app_location: 'dist'
        app_artifact_location: 'dist'

  close_pull_request_app:
    if: github.event_name == 'pull_request' && github.event.action == 'closed'
    runs-on: ubuntu-latest
    name: Close Pull Request App
    steps:
    - name: Close Pull Request
      id: closepullrequest
      uses: Azure/static-web-apps-deploy@v0.0.1-preview
      with:
        azure_static_web_apps_api_token: '${{secrets.STATIC_APP_TOKEN}}'
        action: 'close'
```