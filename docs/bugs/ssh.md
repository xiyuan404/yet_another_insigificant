# Bug report Guide

??? bug "desc"
    Product:	react-router-dom ▾

    Component:	useNavigate Hook ▾
    
    Version:	"react-router-dom": "^6.23.1",
    
    Platform:	Desktop	Macos


- [x]  add the public key to the authorized_keys

```zsh
➜  .ssh nvim /etc/ssh/sshd_config
➜  .ssh chmod 700 ~/.ssh
➜  .ssh touch ~/.ssh/authorized_keys
➜  .ssh chmod 600 ~/.ssh/authorized_keys
➜  .ssh cat id_ed25519.pub >> authorized_keys 
➜  .ssh pwd
/Users/ayao/.ssh
```

- [x] proxy cofig for terminal

```bash
➜ env | grep proxy
https_proxy=http://127.0.0.1:6152
http_proxy=http://127.0.0.1:6152
all_proxy=socks5://127.0.0.1:6153
➜ nvim ~/.zshrc      
➜ source ~/.zshrc
```

- [x] remote access through proxy

```bash
nvim ~/.ssh/config

ProxyCommand /usr/bin/nc -X connect -x 127.0.0.1:6152 %h %p

ssh -o ProxyCommand="nc -X 5 -x 127.0.0.1:1086 %h %p" <remote_user>@<remote_ip>

```

<p>解释：<br>
– <code>-o</code>：option，表示选项的意思；<br>
– <code>ProxyCommand</code>：ssh的一个选项，<code>man ssh</code>的<code>-o</code>选项中可看到有ssh有哪些选项可使用，要进一步看选项具体含义，可用<code>man ssh_config</code>查看；<br>
– <code>nc</code>:netcat，net就是网络，cat就是“concatenate”(连接)，就是连接网络的意思，这是一个工具，可以用<code>man nc</code>查看，windows下应该不存在这个命令，linux也需要安装，mac自带这个命令；<br>
– <code>-X</code>: (大写X)指定代理协议，有三种值：<br>
&nbsp;&nbsp;&nbsp;&nbsp; 4：SOCKS v4<br>
&nbsp;&nbsp;&nbsp;&nbsp; 5：SOCKS v5<br>
&nbsp;&nbsp;&nbsp;&nbsp; connect：HTTPS<br>
– <code>-x</code>：(小写x)指定代理的主机地址和端口，我例子里写<code>127.0.0.1:1086</code>，是因为我用的ss，它有它的本地ip和端口，当然主机地址也可以填域名；<br>
– <code>%h %p</code>：变量，用于替换ssh真正要连接的服务器的主机名(host)和端口(port)；<br>
– <code>root@101.121.218.234</code>：真正要连接的主机。</p>


- [x] update credentials from the macos keychain

> You'll need to update your saved credentials in the `git-credential-osxkeychain` helper if you change your username, password, or personal access token on GitHub

1. Click on the Spotlight icon (magnifying glass) on the right side of the menu bar.
2. Type `Keychain Access`, then press the Enter key to launch the app.
3. In Keychain Access, search for `github.com`.
4. Find the "Internet password" entry for `github.com`.
5. Edit or delete the entry accordingly.