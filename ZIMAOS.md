# ZIMA OS

## Configuração Inicial

- Fazer boot pelo ZimaOS
- Acessar o IP fornecido pelo ZimaOS no boot via navegador
- Ir em **Settings** e configurar:
  - Idioma
  - Search engine: **Startpage**

---

## RAID

Criar RAID combinando 2 ou mais discos via painel de configurações.

---

## Pastas Compartilhadas (Samba)

Referência: https://youtu.be/Vg8leAQgzyU

1. Ir em **Files → Criar pasta → Share via Samba**
2. Adicionar membros e copiar o link gerado pelo Samba
3. No Windows/Mac, digitar usuário e senha para acessar
4. No roteador, fixar o IP do servidor para que não mude

---

## Servidor de Minecraft (Crafty)

1. Instalar **Crafty** com **Custom Install**
2. Clicar no ícone e na seta para baixo `\/`
3. Rolar até o final — em **Volumes** aparecem as pastas do ZimaOS e do Crafty
4. Mudar as pastas para um diretório adequado (criado num HD específico para isso)
5. Selecionar a pasta apenas no primeiro volume (backups -> criar pasta para um HD/SSD)
6. Iniciar o Crafty e acessar via IP no navegador (ignorar aviso de segurança)
7. Localizar a senha padrão em: `crafty/config/default-creds.txt`
8. Fazer login com `admin` + senha copiada
9. **Create New Server**

### Expor o servidor para a internet (playit.gg)

1. Criar conta em [playit.gg](https://playit.gg)
2. Instalar o **playit agent**
3. Antes de executar, ir em **Settings → Logs** e copiar o link gerado
4. Colar o link no navegador → **Continue → Add Agent**
5. **Create Tunnel**:
   - Global Anycast
   - Tunnel Type: **Minecraft Java Game**
6. **Add Tunnel + Enable Tunnel**
7. **Update Local Address**: colocar IP do servidor + porta local padrão do Minecraft
8. Copiar o link público gerado — esse é o endereço do servidor

### Backup automático diário

Ir em **Crafty → Servidor → Schedule → Create New**:

- Nome: `backup diario`
- Action: **Backup Server**
- Backup Policy: **Default Backup**

---

## Immich (tipo Google Fotos)

1. Instalar **Immich (casaos)** com **Custom Install** (selecionar `immich-server`)
2. Em Volumes: a pasta ZimaOS é a pasta "real" no sistema; a pasta Immich é do container
3. Alterar a pasta do ZimaOS para apontar para um HD externo — criar pasta `Immich` lá e selecioná-la
4. Abrir o Immich → Criar conta → Fazer login
5. Configurações: Dark Mode, Language: **pt-BR**
6. Instalar o **Immich no celular**
7. Colocar o IP do servidor com a porta, ex: `192.168.0.101:2283`
8. Fazer login → Permitir acesso às fotos
9. Ir no botão de upload → **Upload All / Tudo**
10. Voltar → **Enable Backup**

---

## Acesso remoto (Tailscale)

1. Instalar **Tailscale** no servidor e criar conta
2. Instalar o **Tailscale no celular** e fazer login
3. Selecionar o servidor (ZimaOS) e copiar o IPv4 gerado
4. Acessar via navegador usando esse IPv4
5. No gerenciador de arquivos do celular, conectar ao servidor usando o link IPv4 + usuário/senha
6. Pronto — acesso às pastas de qualquer lugar do mundo

---

## Pi-hole (bloqueio de anúncios)

1. Instalar o **Pi-hole**
2. Abrir e copiar a senha padrão do ZimaOS → fazer login
3. Ir em **Lists**
4. Abrir o painel do roteador → **DHCP Server → DNS Primário**
5. Colocar o IP do Pi-hole → Salvar
6. Reiniciar o roteador e o navegador (não o servidor)

---

## VMs (ZVM)

1. Abrir o **ZVM** — já vêm algumas "pré-máquinas" disponíveis
2. Baixar uma ISO das disponíveis ou adicionar uma própria
3. **Opções → Create**

> **Dica:** Em `ZVM → Save To`, alterar o diretório de salvamento para um HD externo e não ocupar o SSD.
>
> Referência: https://youtu.be/81tN07ghNs0

---

## Backup do Sistema (Estratégia 3-2-1)

1. Ir no botão **+**
2. Selecionar a pasta raiz do ZimaOS SSD
3. Selecionar destino → criar pasta num HD diferente (ex: `ssd-backups`)
4. Confirmar → **Start** → Aguardar conclusão

---


## Configurações Avançadas

Em **Settings → Apps** é possível alterar:

- AppData
- App Image
- User Database

> O **Zima Client** pode ser desabilitado caso não haja Windows na rede — sem utilidade nesse caso.
>
> Referência: https://youtu.be/SO6_auomzZg
