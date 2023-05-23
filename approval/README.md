# Github Action Approval settings

*You need upgrade to GitHub Enterprise plan if your repository private.*

### 1. Settings [ Repository > Settings > Environments ]

|Param                |Value            | Description      |
|---                  |---              | ---              |
|Name                 |production       |                  |
|Required reviewers   |miumosh          |                  |
|Wait Timer           |[Activate] 30min |                  |
|Allow admin bypass   |[Activate]       |                  |
|Deployment branches  |Selected branches|                  |
|rule-Branch pattern  |production*      |also tag available|
|Environment Secret   |none             |                  |
|Environment Variables|none             |                  |

### 2. Test [ @/.github/workflows/xxx ]
```sh
name: 'Deploy'

on:
  push:
    tags:
      - 'production*'

jobs:
  deploy:
    environment:
      name: production # specify Environment name

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3.0.0
      - run: echo 'Hello'
```