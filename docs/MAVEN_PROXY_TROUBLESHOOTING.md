# Guia de Resolução de Problemas: Maven + Java + Proxy Corporativo

## Problema Comum

Ao usar Maven em ambientes corporativos com proxy, você pode encontrar erros como:

```
status code: 407, reason phrase: Proxy Authentication Required (407)
```

## Soluções

### 1. Configurar o `settings.xml` do Maven

Localização do arquivo:
- **Windows**: `C:\Users\<usuario>\.m2\settings.xml`
- **Linux/Mac**: `~/.m2/settings.xml`
- **Global**: `<MAVEN_HOME>/conf/settings.xml`

```xml
<settings>
    <proxies>
        <!-- Proxy para HTTPS (obrigatório para Maven Central) -->
        <proxy>
            <id>https-proxy</id>
            <active>true</active>
            <protocol>https</protocol>
            <host>10.1.10.5</host>
            <port>3128</port>
            <username>seu.usuario</username>
            <password>sua.senha</password>
            <nonProxyHosts>localhost|127.0.0.1|10.*|*.local</nonProxyHosts>
        </proxy>
        
        <!-- Proxy para HTTP (opcional, mas recomendado) -->
        <proxy>
            <id>http-proxy</id>
            <active>true</active>
            <protocol>http</protocol>
            <host>10.1.10.5</host>
            <port>3128</port>
            <username>seu.usuario</username>
            <password>sua.senha</password>
            <nonProxyHosts>localhost|127.0.0.1|10.*|*.local</nonProxyHosts>
        </proxy>
    </proxies>
</settings>
```

### 2. Configurar a Variável `MAVEN_OPTS`

O Java por padrão bloqueia autenticação através de túneis HTTPS (segurança). Para habilitar:

**Windows (PowerShell):**
```powershell
$env:MAVEN_OPTS = "-Djdk.http.auth.tunneling.disabledSchemes="
```

**Windows (CMD):**
```cmd
set MAVEN_OPTS=-Djdk.http.auth.tunneling.disabledSchemes=
```

**Linux/Mac:**
```bash
export MAVEN_OPTS="-Djdk.http.auth.tunneling.disabledSchemes="
```

### 3. Configuração Permanente

#### Windows - Variável de Ambiente do Sistema

1. Abra "Variáveis de Ambiente do Sistema"
2. Em "Variáveis do Sistema", clique em "Novo"
3. Nome: `MAVEN_OPTS`
4. Valor: `-Djdk.http.auth.tunneling.disabledSchemes=`

#### Linux/Mac - Adicionar ao `.bashrc` ou `.zshrc`

```bash
echo 'export MAVEN_OPTS="-Djdk.http.auth.tunneling.disabledSchemes="' >> ~/.bashrc
source ~/.bashrc
```

### 4. Script PowerShell para Projetos Java

Crie um arquivo `run-maven.ps1` na raiz do projeto:

```powershell
# run-maven.ps1
# Configura ambiente para Maven com proxy corporativo

# Configurar Java 8 (ajuste o caminho conforme sua instalação)
$env:JAVA_HOME = "C:\Program Files\Java\jdk1.8.0_191"
$env:PATH = "$env:JAVA_HOME\bin;$env:PATH"

# Habilitar autenticação através de proxy HTTPS
$env:MAVEN_OPTS = "-Djdk.http.auth.tunneling.disabledSchemes="

# Executar Maven com os argumentos passados
mvn @args --no-transfer-progress
```

Uso:
```powershell
.\run-maven.ps1 clean install
.\run-maven.ps1 test -Dtest=MinhaClasseTest
```

### 5. Comando Completo (One-liner)

**PowerShell:**
```powershell
$env:JAVA_HOME = "C:\Program Files\Java\jdk1.8.0_191"; $env:PATH = "$env:JAVA_HOME\bin;$env:PATH"; $env:MAVEN_OPTS = "-Djdk.http.auth.tunneling.disabledSchemes="; mvn clean install --no-transfer-progress
```

## Flags Úteis do Maven

| Flag | Descrição |
|------|-----------|
| `--no-transfer-progress` | Suprime mensagens de progresso de download |
| `-o` ou `--offline` | Modo offline (usa apenas cache local) |
| `-U` | Força atualização de snapshots |
| `-q` | Modo silencioso (menos output) |

## Verificação

Para verificar se o proxy está funcionando:

```bash
mvn help:effective-settings
```

Isso mostrará as configurações efetivas, incluindo proxy.

## Problemas Comuns

### Erro: `407 Proxy Authentication Required`
- Verifique usuário/senha no `settings.xml`
- Adicione `MAVEN_OPTS` com `jdk.http.auth.tunneling.disabledSchemes=`

### Erro: `PKIX path building failed`
- O proxy pode estar interceptando SSL
- Importe o certificado do proxy no keystore Java:
```bash
keytool -import -alias proxy-cert -file proxy.crt -keystore $JAVA_HOME/jre/lib/security/cacerts
```

### Erro: `Connection timed out`
- Verifique se o host/porta do proxy estão corretos
- Teste conectividade: `telnet <proxy-host> <proxy-port>`

### Maven funciona mas IDE não
- Configure o proxy também na IDE (IntelliJ, Eclipse, VS Code)
- IntelliJ: `File > Settings > Appearance & Behavior > System Settings > HTTP Proxy`

## Referências

- [Maven Proxy Settings](https://maven.apache.org/guides/mini/guide-proxies.html)
- [JDK HTTP Tunneling](https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html)
