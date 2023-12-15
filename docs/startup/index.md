---
hide:
  # - navigation
  - toc
---

为了开始使用 mihomo, 你可以从以下安装方式中选择一个为当前系统安装 mihomo

## 使用预编译的二进制文件

请在下方选择下载常见的操作系统对应的二进制文件

=== "Windows"
    === "amd64/x86_64"
        <div id="download-section">
          <div id="windows-amd64">
            <ul id="download-list-windows-amd64">加载中...</ul>
          </div>
        </div>
    === "386/x86_32"
        <div id="download-section">
          <div id="windows-386">
            <ul id="download-list-windows-386">加载中...</ul>
          </div>
        </div>
    === "arm64/armv8"
        <div id="download-section">
          <div id="windows-arm64">
            <ul id="download-list-windows-arm64">加载中...</ul>
          </div>
        </div>
    === "armv7"
        <div id="download-section">
          <div id="windows-arm32v7">
            <ul id="download-list-windows-arm32v7">加载中...</ul>
          </div>
        </div>

=== "Linux"
    === "amd64/x86_64"
        <div id="download-section">
          <div id="linux-amd64">
            <ul id="download-list-linux-amd64">加载中...</ul>
          </div>
        </div>
    === "386/x86_32"
        <div id="download-section">
          <div id="linux-386">
            <ul id="download-list-linux-386">加载中...</ul>
          </div>
        </div>
    === "arm64/armv8"
        <div id="download-section">
          <div id="linux-arm64">
            <ul id="download-list-linux-arm64">加载中...</ul>
          </div>
        </div>
    === "armv7"
        <div id="download-section">
          <div id="linux-armv7">
            <ul id="download-list-linux-armv7">加载中...</ul>
          </div>
        </div>
    === "riscv64"
        <div id="download-section">
          <div id="linux-riscv64">
            <ul id="download-list-linux-riscv64">加载中...</ul>
          </div>
        </div>
    === "mips"
        <div id="download-section">
          <div id="linux-mips">
            <ul id="download-list-linux-mips">加载中...</ul>
          </div>
        </div>

=== "MacOS"
    === "amd64/x86_64"
        <div id="download-section">
          <div id="darwin-amd64">
            <ul id="download-list-darwin-amd64">加载中...</ul>
          </div>
        </div>
    === "arm64/armv8"
        <div id="download-section">
          <div id="darwin-arm64">
            <ul id="download-list-darwin-arm64">加载中...</ul>
          </div>
        </div>

=== "FreeBSD"
    === "amd64/x86_64"
        <div id="download-section">
          <div id="freebsd-amd64">
            <ul id="download-list-freebsd-amd64">加载中...</ul>
          </div>
        </div>
    === "386/x86_32"
        <div id="download-section">
          <div id="freebsd-386">
            <ul id="download-list-freebsd-386">加载中...</ul>
          </div>
        </div>
    === "arm64/armv8"
        <div id="download-section">
          <div id="freebsd-arm64">
            <ul id="download-list-freebsd-arm64">加载中...</ul>
          </div>
        </div>

=== "Android"
    === "arm64"
        <div id="download-section">
          <div id="android-arm64">
            <ul id="download-list-android-arm64">加载中...</ul>
          </div>
        </div>


<script>
  const downloadSections = {
    'windows-amd64': document.getElementById('download-list-windows-amd64'),
    'windows-386': document.getElementById('download-list-windows-386'),
    'windows-arm64': document.getElementById('download-list-windows-arm64'),
    'windows-arm32v7': document.getElementById('download-list-windows-arm32v7'),
    'linux-amd64': document.getElementById('download-list-linux-amd64'),
    'linux-386': document.getElementById('download-list-linux-386'),
    'linux-arm64': document.getElementById('download-list-linux-arm64'),
    'linux-armv7': document.getElementById('download-list-linux-armv7'),
    'linux-riscv64': document.getElementById('download-list-linux-riscv64'),
    'linux-mips': document.getElementById('download-list-linux-mips'),
    'darwin-amd64': document.getElementById('download-list-darwin-amd64'),
    'darwin-arm64': document.getElementById('download-list-darwin-arm64'),
    'freebsd-amd64': document.getElementById('download-list-freebsd-amd64'),
    'freebsd-386': document.getElementById('download-list-freebsd-386'),
    'freebsd-arm64': document.getElementById('download-list-freebsd-arm64'),
    'android-arm64': document.getElementById('download-list-android-arm64'),
  }

  const fileList = []
  const githubLink = 'https://github.com/MetaCubeX/mihomo/releases'

  const getFileList = async () => {
    const link = 'https://api.github.com/repos/MetaCubeX/mihomo/releases/tags/Prerelease-Alpha'
    const { assets } = await fetch(link).then(r => r.json())
    assets.forEach(({ name, browser_download_url: url }) => {
      fileList.push({ name, url })
    })
  }

  const updateDownloadLinks = () => {
    for (const sectionId in downloadSections) {
      const section = downloadSections[sectionId]
      section.innerHTML = ''

      const filteredFiles = fileList.filter(({ name }) => name.includes(sectionId))

      filteredFiles.forEach(({ name, url }) => {
        const listItem = document.createElement('li')
        const link = document.createElement('a')
        link.textContent = name
        link.download = name
        link.href = url
        listItem.appendChild(link)
        section.appendChild(listItem)
      })

      section.style.display = filteredFiles.length > 0 ? 'block' : 'none'
    }
  }

  getFileList().then(() => {
    updateDownloadLinks()
  }, () => {
    for (const sectionId in downloadSections) {
      downloadSections[sectionId].innerHTML = `加载失败，您可以在 github 下载 mihomo 的内核二进制文件： <a href="${githubLink}" target="_blank">github release</a>`
    }
  })
</script>
