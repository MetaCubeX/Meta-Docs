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


  <label for="os">Select Operating System:</label>
  <select id="os">
    <option value="linux">Linux</option>
    <option value="windows">Windows</option>
    <option value="darwin">MacOS</option>
    <!-- Add more options as needed -->
  </select>

  <label for="arch">Select Architecture:</label>
  <select id="arch">
    <option value="amd64">amd64</option>
    <option value="arm">arm</option>
    <!-- Add more options as needed -->
  </select>

  <div id="download-section">
    <h2>Download Links:</h2>
    <ul id="download-list"></ul>
  </div>

  <script>
    var downloadList = document.getElementById("download-list");
    var osSelect = document.getElementById("os");
    var archSelect = document.getElementById("arch");

    var fileList = [
      "mihomo-darwin-amd64-alpha-0123456.gz",
      "mihomo-freebsd-amd64-alpha-0123456.gz",
      "mihomo-linux-arm64-alpha-0123456.gz",
      "mihomo-darwin-arm64-alpha-0123456.gz",
      "mihomo-android-arm64-alpha-0123456.gz",
      "mihomo-freebsd-arm64-alpha-0123456.gz",
      "mihomo-linux-mips-softfloat-alpha-0123456.gz",
      "mihomo-linux-amd64-go120-alpha-0123456.gz",
      "mihomo-linux-armv5-alpha-0123456.gz",
      "mihomo-darwin-amd64-go120-alpha-0123456.gz",
      "mihomo-linux-amd64-compatible-alpha-0123456.gz",
      "mihomo-linux-mips64-alpha-0123456.gz",
      "mihomo-linux-amd64-compatible-go120-alpha-0123456.gz",
      "mihomo-darwin-arm64-go120-alpha-0123456.gz",
      "mihomo-windows-386-alpha-0123456.zip",
      "mihomo-freebsd-386-alpha-0123456.gz",
      "mihomo-windows-amd64-compatible-alpha-0123456.zip",
      "mihomo-android-arm64-go120-alpha-0123456.gz",
      "mihomo-windows-arm64-alpha-0123456.zip",
      "mihomo-linux-386-alpha-0123456.gz",
      "mihomo-windows-amd64-alpha-0123456.zip",
      "mihomo-linux-mips64le-alpha-0123456.gz",
      "mihomo-windows-amd64-go120-alpha-0123456.zip",
      "mihomo-windows-amd64-compatible-go120-alpha-0123456.zip",
      "mihomo-linux-armv7-alpha-0123456.gz",
      "mihomo-linux-amd64-alpha-0123456.gz",
      "mihomo-linux-armv6-alpha-0123456.gz",
      "mihomo-linux-riscv64-alpha-0123456.gz",
      "mihomo-linux-mipsle-hardfloat-alpha-0123456.gz",
      "mihomo-linux-mips-hardfloat-alpha-0123456.gz",
      "mihomo-windows-386-go120-alpha-0123456.zip",
      "mihomo-linux-mipsle-softfloat-alpha-0123456.gz",
      "mihomo-windows-arm32v7-alpha-0123456.zip",
    ];

    function updateDownloadLinks() {
      fetch('https://github.com/MetaCubeX/mihomo/releases/download/Prerelease-Alpha/version.txt')
        .then(res => res.text()) 
        .then(data => {
      const version = data.match(/alpha-(\w+)/)[1];

      // 更新文件名
      fileList.forEach(file => {
        file = file.replace(/alpha-\w+/, `alpha-${version}`);  
      });
    });
      var os = osSelect.value;
      var arch = archSelect.value;

      // Filter files based on user selection
      var filteredFiles = fileList.filter(function (file) {
        return file.includes(os) && file.includes(arch);
      });

      // Display the download links
      var downloadList = document.getElementById("download-list");
      downloadList.innerHTML = "";
      filteredFiles.forEach(function (file) {
        var listItem = document.createElement("li");
        listItem.textContent = file;
        downloadList.appendChild(listItem);
      });

      // Show the download section
      var downloadSection = document.getElementById("download-section");
      downloadSection.style.display = "block";
    }

    // Attach the updateDownloadLinks function to the change event of the architecture dropdown
    osSelect.addEventListener("change", updateDownloadLinks);
    archSelect.addEventListener("change", updateDownloadLinks);

    // Optionally, you can call updateDownloadLinks initially if you want to show the links immediately
    updateDownloadLinks();
  </script>