# Same workflow each env
### Composition


### if
```sh
jobs:
  job-name:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set env for dev
        if: contains(toJSON(github.ref), 'dev')
        run: echo env=dev >> $GITHUB_ENV

      - name: Set env for stg
        if: contains(toJSON(github.ref), 'stg')
        run: echo env=stg >> $GITHUB_ENV

      - name: Set env for prd
        if: contains(toJSON(github.ref), 'prd')
        run: echo env=prd >> $GITHUB_ENV

# using by ${{env.ENVIRONMENT}}
```


### Script
```sh
# ./.github/workflows/xxx.yml
jobs:
  job-name:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Get branch name
        shell: bash
        run: echo "branchName=$(echo ${GITHUB_REF#refs/heads/})" >> $GITHUB_ENV

      - name: Set env from sh file
        run: |
          chmod +x .github/workflows/${{ env.branchName }}.sh
          ./.github/workflows/${{ env.branchName }}.sh

# ./.github/workflows/xxx.sh

```


# Settings that run outside of certain environments
```sh
# only dev execute
   name: Build Non prod
    needs: [rules]    
    if: ${{ (needs.rules.outputs.branch_name != 'prd') && (needs.rules.outputs.branch_name != 'stg') }}
    steps:
      - name: Checkout
        uses: actions/checkout@v2
```

# 三項演算子
```sh
# (条件 && 条件がtrueの場合の値) || 条件がfalseの場合の値
jobs:
  test:
    name: Test
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: Test
        uses: ./
        with:
          test: ${{ (github.event_name == 'pull_request' && 'test1') || 'test2' }}
```