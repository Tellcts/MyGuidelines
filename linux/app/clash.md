# Clash-Verge-Rev下载指南



## 1. GitHub-CLI 

- **RPM系**

  ~~~bash
  gh release download --repo clash-verge-rev/clash-verge-rev -p '*.x86_64.rpm' -D ~/下载
  ~~~

- **DEB系**

  ~~~bash
  gh release download --repo clash-verge-rev/clash-verge-rev -p '*amd64.deb' -D ~/下载
  ~~~

- **如果不确定Release labels,可以运行以下命令进行查看，根据系统版本将名称填入模式参数之后：**

  ~~~bash
  # examples
  gh release view --repo clash-verge-rev/clash-verge-rev
  
  gh release download --repo clash-verge-rev/clash-verge-rev -p 'Clash.Verge_2.5.1_x64-setup.exe' -D ~/下载
  ~~~

  