---
hide:
  # - navigation
  - toc
---

To start using Mihomo, you can choose one of the following installation methods for your current system.

## Using Precompiled Binary Files

Please select and download the binary file corresponding to your common operating system from the options below.

=== "Windows"
    === "amd64/x86_64"
        <div class="download-list" data-keyword="windows-amd64"><ul>加载中...</ul></div>
    === "386/x86_32"
        <div class="download-list" data-keyword="windows-386"><ul>加载中...</ul></div>
    === "arm64/armv8"
        <div class="download-list" data-keyword="windows-arm64"><ul>加载中...</ul></div>
    === "armv7"
        <div class="download-list" data-keyword="windows-arm32v7"><ul>加载中...</ul></div>

=== "Linux"
    === "amd64/x86_64"
        <div class="download-list" data-keyword="linux-amd64"><ul>加载中...</ul></div>
    === "386/x86_32"
        <div class="download-list" data-keyword="linux-386"><ul>加载中...</ul></div>
    === "arm64/armv8"
        <div class="download-list" data-keyword="linux-arm64"><ul>加载中...</ul></div>
    === "armv7"
        <div class="download-list" data-keyword="linux-armv7"><ul>加载中...</ul></div>
    === "riscv64"
        <div class="download-list" data-keyword="linux-riscv64"><ul>加载中...</ul></div>
    === "mips"
        <div class="download-list" data-keyword="linux-mips"><ul>加载中...</ul></div>

=== "MacOS"
    === "amd64/x86_64"
        <div class="download-list" data-keyword="darwin-amd64"><ul>加载中...</ul></div>
    === "arm64/armv8"
        <div class="download-list" data-keyword="darwin-arm64"><ul>加载中...</ul></div>

=== "FreeBSD"
    === "amd64/x86_64"
        <div class="download-list" data-keyword="freebsd-amd64"><ul>加载中...</ul></div>
    === "386/x86_32"
        <div class="download-list" data-keyword="freebsd-386"><ul>加载中...</ul></div>
    === "arm64/armv8"
        <div class="download-list" data-keyword="freebsd-arm64"><ul>加载中...</ul></div>

=== "Android"
    === "arm64"
        <div class="download-list" data-keyword="android-arm64"><ul>加载中...</ul></div>

<script>
  const fileList = []
  const divList = document.querySelectorAll('div.download-list')
  const githubLink = 'https://github.com/MetaCubeX/mihomo/releases'

  const getFileList = async () => {
    const link = 'https://api.github.com/repos/MetaCubeX/mihomo/releases/tags/Prerelease-Alpha'
    const { assets } = await fetch(link).then(r => r.json())
    for (const { name, browser_download_url: url } of assets) fileList.push({ name, url })
  }

  getFileList().then(() => {
    for (const div of divList) {
      const keyword = div.getAttribute('data-keyword')
      const ul = div.querySelector('ul')
      ul.innerHTML = ''
      for (const { name, url } of fileList) {
        if (!name.includes(keyword)) continue
        const a = document.createElement('a')
        const li = document.createElement('li')
        a.href = url
        a.download = name
        a.innerText = name
        li.appendChild(a)
        ul.appendChild(li)
      }
    }
  }, () => {
    for (const div of divList) {
      const ul = div.querySelector('ul')
      ul.innerHTML = `加载失败，您可以在 github 下载 mihomo 的内核二进制文件： <a href="${githubLink}" target="_blank">github release</a>`
    }
  })
</script>
