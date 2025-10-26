[![Test](https://github.com/ollydev/setup-lazarus/actions/workflows/test.yml/badge.svg)](https://github.com/ollydev/setup-lazarus/actions/workflows/test.yml)

Simple action which installs Lazarus and FPC from sourceforge releases.

## Example usage

```yml
on: [push, pull_request]

jobs:
  test:
    name: ${{ matrix.config.name }}
    runs-on: ${{ matrix.config.os }}
    defaults:
      run:
        shell: bash
    strategy:
      fail-fast: false
      matrix:
        config:
          - os: windows-latest
            name: Windows 64
            laz-url: https://sourceforge.net/projects/lazarus/files/Lazarus Windows 64 bits/Lazarus 4.2/lazarus-4.2-fpc-3.2.2-win64.exe
            
          - os: windows-latest
            name: Windows 32
            laz-url: https://sourceforge.net/projects/lazarus/files/Lazarus Windows 32 bits/Lazarus 4.2/lazarus-4.2-fpc-3.2.2-win32.exe

          - os: ubuntu-latest
            name: Linux 64
            laz-url: https://sourceforge.net/projects/lazarus/files/Lazarus Linux amd64 DEB/Lazarus 4.2/lazarus-project_4.2.0-0_amd64.deb
            fpc-url: |
              https://sourceforge.net/projects/lazarus/files/Lazarus Linux amd64 DEB/Lazarus 4.2/fpc-laz_3.2.2-210709_amd64.deb
              https://sourceforge.net/projects/lazarus/files/Lazarus Linux amd64 DEB/Lazarus 4.2/fpc-src_3.2.2-210709_amd64.deb
            
          - os: macos-latest
            name: MacOS 64
            laz-url: https://sourceforge.net/projects/lazarus/files/Lazarus macOS x86-64/Lazarus 4.2/lazarus-darwin-x86_64-4.2.zip
            fpc-url: https://sourceforge.net/projects/lazarus/files/Lazarus macOS x86-64/Lazarus 4.2/fpc-3.2.2.intelarm64-macosx.dmg
        
    steps:
      - uses: actions/checkout@v5
      
      - name: Install Lazarus
        uses: ollydev/setup-lazarus@v2
        with:
          laz-url: ${{ matrix.config.laz-url }}
          fpc-url: ${{ matrix.config.fpc-url }}
      
      - name: Test Installation
        run: |
          lazbuild --version
```
