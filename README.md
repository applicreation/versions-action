# versions-action

## Usage

```yaml
on:
  release:
    types:
      - published
jobs:
  release:
    runs-on: ubuntu-latest
    steps:  
      - uses: applicreation/versions-action@v2
        id: versions
        with:
          tag: ${{ github.ref_name }}
      - run: echo ${{ steps.versions.outputs.has_prefix }}
      - run: echo ${{ steps.versions.outputs.major }}
      - run: echo ${{ steps.versions.outputs.minor }}
      - run: echo ${{ steps.versions.outputs.patch }}
```
