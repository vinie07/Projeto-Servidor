# ZIMA OS

# Configuração Inicial

-   Fazer boot pelo ZimaOS
    
-   Acessar o IP fornecido pelo ZimaOS no boot via navegador
    
-   Ir em **Settings** e configurar:
    
    -   Idioma
        
    -   Search engine: **Startpage**
        

* * *

# RAID

Criar RAID combinando 2 ou mais discos via painel de configurações.

Ao criar o RAID, ele entra em estado de **ressincronização**

Evite desligar o servidor no meio da sincronização se possível.

* * *

# Pastas Compartilhadas (Samba)

Referência: https://youtu.be/Vg8leAQgzyU

1.  Ir em **Files → Criar pasta → Share via Samba**
    
2.  Adicionar membros e copiar o link gerado pelo Samba
    
3.  No Windows/Mac, digitar usuário e senha para acessar
    
4.  No roteador, fixar o IP do servidor para que não mude
    

* * *

# Servidor de Minecraft (Crafty)

1.  Instalar **Crafty** com **Custom Install**
    
2.  Clicar no ícone e na seta para baixo `\/`
    
3.  Rolar até o final — em **Volumes** aparecem as pastas do ZimaOS e do Crafty
    
4.  Mudar as pastas para um diretório adequado (criado num HD específico para isso)
    
5.  Selecionar a pasta apenas no primeiro volume (backups -> criar pasta para um HD/SSD)
    
6.  Iniciar o Crafty e acessar via IP no navegador (ignorar aviso de segurança)
    
7.  Localizar a senha padrão em: `crafty/config/default-creds.txt`
    
8.  Fazer login com `admin` + senha copiada
    
9.  **Create New Server**
    

# Expor o servidor para a internet (playit.gg)

1.  Criar conta em [playit.gg](https://playit.gg)
    
2.  Instalar o **playit agent**
    
3.  Antes de executar, ir em **Settings → Logs** e copiar o link gerado
    
4.  Colar o link no navegador → **Continue → Add Agent**
    
5.  **Create Tunnel**:
    
    -   Global Anycast
        
    -   Tunnel Type: **Minecraft Java Game**
        
6.  **Add Tunnel + Enable Tunnel**
    
7.  **Update Local Address**: colocar IP do servidor + porta local padrão do Minecraft
    
8.  Copiar o link público gerado — esse é o endereço do servidor
    

# Suporte a Bedrock (GeyserMC + Floodgate)

Permite que jogadores do Minecraft Bedrock (celular, console) entrem no servidor Java.

**Instalação:**

1.  Baixar o **GeyserMC** (plugin/mod) e o **Floodgate** e colocar na pasta `plugins/` do servidor
    
2.  Reiniciar o servidor para gerar os arquivos de config
    

**Configuração do** `config.yml` **do Geyser:**

```yaml
bedrock:
  address: 0.0.0.0   # obrigatório — não usar 127.0.0.1
  port: 19132

remote:
  address: 127.0.0.1
  port: 25565
  auth-type: floodgate  # tem que ser floodgate, não online
```

**Porta no Docker (Crafty yaml):**

```yaml
ports:
  - "19132:19132/udp"  # /udp é obrigatório — sem isso o Bedrock não conecta
```

**Túnel Bedrock no playit.gg:**

-   Criar um túnel **separado** do Java, com tipo **Minecraft Bedrock** (UDP)
    
-   No campo **Local IP**, colocar `172.17.0.1` (gateway Docker) — **não o IP da LAN** (`192.168.0.x`)
    
    -   O playit roda num container na bridge `172.17.x.x` e o Crafty em outra (`172.18.x.x`)
        

# Instalar Java manualmente no ZimaOS (para servidores que precisam de Java diferente)

O ZimaOS não tem `apt` e o home é somente-leitura. O Java precisa ir num volume visível pelo container do Crafty.

```bash
# Baixar o JDK direto da Adoptium (ajustar o número da versão conforme necessário)
curl -L "https://api.adoptium.net/v3/binary/latest/25/ga/linux/x64/jdk/hotspot/normal/eclipse" \
  -o /tmp/jdk25.tar.gz

# Extrair
cd /tmp && tar -xzf jdk25.tar.gz

# Mover para dentro do volume do Crafty (caminho real no ZimaOS → caminho no container)
# Exemplo: /media/ZimaOS-HD/MINES/servidores/ → /crafty/servers/ dentro do container
mv /tmp/jdk-25.* /media/ZimaOS-HD/MINES/servidores/jdk-25
```

No Crafty, trocar o Java do servidor para:

```
/crafty/servers/jdk-25/bin/java
```

Comando de execução completo:

```
/crafty/servers/jdk-25/bin/java -Xms1024M -Xmx4096M -jar fabric-server.jar nogui
```

**Referência de versões:** Java 21 = class file 65.0 / Java 25 = class file 69.0.

O erro `class file version X` indica qual Java o servidor exige.

# Backup automático diário

Ir em **Crafty → Servidor → Schedule → Create New**:

-   Nome: `backup diario`
    
-   Action: **Backup Server**
    
-   Backup Policy: **Default Backup**
    

* * *

# Immich (tipo Google Fotos)

1.  Instalar **Immich (casaos)** com **Custom Install** (selecionar `immich-server`)
    
2.  Em Volumes: a pasta ZimaOS é a pasta "real" no sistema; a pasta Immich é do container
    
3.  Alterar a pasta do ZimaOS para apontar para um HD externo — criar pasta `Immich` lá e selecioná-la
    
4.  Abrir o Immich → Criar conta → Fazer login
    
5.  Configurações: Dark Mode, Language: **pt-BR**
    
6.  Instalar o **Immich no celular**
    
7.  Colocar o IP do servidor com a porta, ex: `192.168.0.101:2283`
    
8.  Fazer login → Permitir acesso às fotos
    
9.  Ir no botão de upload → **Upload All / Tudo**
    
10.  Voltar → **Enable Backup**
     

* * *

# Acesso remoto (Tailscale)

1.  Instalar **Tailscale** no servidor e criar conta
    
2.  Instalar o **Tailscale no celular** e fazer login
    
3.  Selecionar o servidor (ZimaOS) e copiar o IPv4 gerado
    
4.  Acessar via navegador usando esse IPv4
    
5.  No gerenciador de arquivos do celular, conectar ao servidor usando o link IPv4 + usuário/senha
    
6.  Pronto — acesso às pastas de qualquer lugar do mundo
    

* * *

# Pi-hole (bloqueio de anúncios)

1.  Instalar o **Pi-hole**
    
2.  Abrir e copiar a senha padrão do ZimaOS → fazer login
    
3.  Ir em **Lists**
    
4.  Abrir o painel do roteador → **DHCP Server → DNS Primário**
    
5.  Colocar o IP do Pi-hole → Salvar
    
6.  Reiniciar o roteador e o navegador (não o servidor)
    

**Anotar o DNS original antes de trocar** — caso algo dê errado, restaurar aqui.

* * *

# VMs (ZVM)

1.  Abrir o **ZVM** — já vêm algumas "pré-máquinas" disponíveis
    
2.  Baixar uma ISO das disponíveis ou adicionar uma própria
    
3.  **Opções → Create**
    

Em `ZVM → Save To`, alterar o diretório de salvamento para um HD externo e não ocupar o SSD.

Referência: https://youtu.be/81tN07ghNs0

* * *

# Backup do Sistema (Estratégia 3-2-1)

1.  Ir no botão **+**
    
2.  Selecionar a pasta raiz do ZimaOS SSD
    
3.  Selecionar destino → criar pasta num HD diferente (ex: `ssd-backups`)
    
4.  Confirmar → **Start** → Aguardar conclusão
    

* * *

# Configurações Avançadas

Em **Settings → Apps** é possível alterar:

-   AppData
    
-   App Image
    
-   User Database
    

O **Zima Client** pode ser desabilitado caso não haja Windows na rede — sem utilidade nesse caso.

Referência: https://youtu.be/SO6\_auomzZg
