---
hide:
  # - navigation
  - toc
---

为了开始使用 Clash.Meta ,你可以从以下安装方式中选择一个为当前系统安装 Clash.Meta

## 使用预编译的二进制文件

您可以在这里下载 Clash.Meta 的内核二进制文件：<https://github.com/MetaCubeX/mihomo/releases>

---

常见的操作系统与对应的二进制文件：


=== "Windows"
    === "amd64/x86_64"
        === "amd64 v1"
            windows-amd64-compatible
        === "amd64 v3"
            windows-amd64
    === "386/x86_32"
        windows-386
    === "arm64/armv8"
        windows-arm64
    === "armv7"
        windows-armv7

=== "Linux"
    === "amd64/x86_64"
        === "amd64 v1"
            linux-amd64-compatible
        === "amd64 v3"
            linux-amd64
    === "386/x86_32"
        linux-386
    === "arm64/armv8"
        linux-arm64
    === "armv7"
        linux-armv7
    === "riscv64"
        linux-riscv64

=== "MacOS"
    === "amd64/x86_64"
        darwin-amd64
    === "arm64/armv8"
        darwin-arm64

=== "FreeBSD"
    === "amd64/x86_64"
        freebsd-amd64
    === "386/x86_32"
        freebsd-386
    === "arm64/armv8"
        freebsd-arm64

=== "Android"
    === "arm64"
        android-arm64

上面是画饼.先用下面的


  <label for="os">Select Operating System:</label>
  <select id="os" disabled>
    <option value="linux">Linux</option>
    <option value="windows">Windows</option>
    <option value="darwin">MacOS</option>
    <!-- Add more options as needed -->
  </select>

  <label for="arch">Select Architecture:</label>
  <select id="arch" disabled>
    <option value="amd64">amd64</option>
    <option value="arm">arm</option>
    <!-- Add more options as needed -->
  </select>

  <div id="download-section">
    <h2>Avaiable files:</h2>
    <ul id="download-list">Loading...</ul>
  </div>

  <script>
  {
    const downloadList = document.getElementById('download-list');
    const osSelect = document.getElementById('os');
    const archSelect = document.getElementById('arch');

    const fileList = [];

    const getFileList = async () => {
      const link = 'https://api.github.com/repos/MetaCubeX/mihomo/releases/tags/Prerelease-Alpha'
      const {assets} = await fetch(link).then(r => r.json());
      for (const {name, browser_download_url: url} of assets) {
        if (name === 'version.txt') continue;
        fileList.push({name, url});
      }
    }

    const updateDownloadLinks = () => {
      const os = osSelect.value;
      const arch = archSelect.value;

      // Filter files based on user selection
      const filteredFiles = fileList.filter(({name}) => {
        return name.includes(os) && name.includes(arch);
      });

      // Display the download links
      const downloadList = document.getElementById('download-list');
      downloadList.innerHTML = '';
      for (const {name, url} of filteredFiles) {
        const listItem = document.createElement('li');
        const button = document.createElement('a')
        button.textContent = name;
        button.download = name;
        button.href = url;
        listItem.appendChild(button)
        downloadList.appendChild(listItem);
      }

      // Show the download section
      const downloadSection = document.getElementById('download-section');
      downloadSection.style.display = 'block';
    }

    getFileList().then(() => {
      // Enable select items when fetch is complete
      osSelect.removeAttribute('disabled');
      archSelect.removeAttribute('disabled');
      // Optionally, you can call updateDownloadLinks initially if you want to show the links immediately
      updateDownloadLinks()
      // Attach the updateDownloadLinks function to the change event of the architecture dropdown
      osSelect.addEventListener('change', updateDownloadLinks);
      archSelect.addEventListener('change', updateDownloadLinks);
    }, () => {
      downloadList.innerHTML = 'Load Failed';
    });
  }
  </script>