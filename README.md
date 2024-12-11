# Configuração de Impressoras Epson TM-20 e Elgin i9 com CUPS no Raspberry Pi

Este guia descreve o processo de instalação e configuração das impressoras Epson TM-t20 e Elgin i9 no Raspberry Pi utilizando o CUPS para gerenciamento de impressão.

---

## Pré-requisitos

1. **Hardware Necessário**:
   - Raspberry Pi com acesso à internet.
   - Impressora Epson TM-20 ou Elgin i9 conectada via USB.

2. **Software Necessário**:
   - Sistema operacional Linux (ex.: Raspberry Pi OS).
   - CUPS (Common UNIX Printing System).

3. **Drivers e Arquivos**:
   - Arquivo `.ppd`  (incluso no repositório)
   - Pacote `.deb`  (incluso no repositório).

---

## Passo 1: Instalar o CUPS

1. Atualize o sistema:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. Instale o CUPS:
   ```bash
   sudo apt install cups -y
   ```

3. Adicione o usuário atual ao grupo `lpadmin` para permissão de administração de impressoras:
   ```bash
   sudo usermod -a -G lpadmin pi
   ```

4. Habilite o acesso remoto ao CUPS:
   ```bash
   sudo cupsctl --remote-any
   ```

5. Reinicie o serviço do CUPS:
   ```bash
   sudo systemctl restart cups
   ```

6. Acesse o painel de administração do CUPS no navegador:
   ```
   http://<IP_DO_RASPBERRY>:631
   ```

---

## Passo 2: Configurar os Drivers

1. Clone este repositório no Raspberry Pi:
   ```bash
   git clone https://github.com/datasilver/epsontm20-elgini9rasp.git
   cd epsontm20-elgini9rasp
   ```

2. Copie os arquivos `.ppd` para o diretório padrão do CUPS:
   ```bash
   sudo cp drivers/*.ppd /usr/share/cups/model/
   ```

3. (Opcional) Instale o driver `.deb` para a impressora:
   ```bash
   sudo dpkg -i drivers/i9etm20.deb
   ```

4. Resolva dependências, se necessário:
   ```bash
   sudo apt-get install -f
   ```

---

## Passo 3: Adicionar a Impressora

1. Conecte a impressora ao Raspberry Pi.

2. Acesse o painel do CUPS:
   ```
   http://<IP_DO_RASPBERRY>:631
   ```

3. Clique em **"Administration"** > **"Add Printer"**.

4. Selecione a impressora conectada **"Unknow"** e clique em **"Continue"**.

5. Escolha o driver correto:
   ELGIN - > I9 

6. Configure as preferências, como tamanho do papel e margens.

---

## Passo 4: Compartilhar a Impressora na Rede (OPICIONAL)

1. Certifique-se de que o compartilhamento de impressoras esteja habilitado:
   - Acesse o painel do CUPS.
   - Clique em **"Printers"**.
   - Selecione a impressora configurada.
   - Marque a opção **"Compartilhar esta impressora"**.

2. Verifique o endereço IP do Raspberry Pi:
   ```bash
   hostname -I
   ```

3. No Windows, adicione a impressora compartilhada:
   - Use o endereço:
     ```
     http://<IP_DO_RASPBERRY>:631/printers/<NOME_DA_IMPRESSORA>
     ```

---

## Arquivos Incluídos no Repositório

- `drivers/tm20.ppd`: Driver PPD para Epson TM-20.
- `drivers/i9.ppd`: Driver PPD para Elgin i9.
- `drivers/Elgin_i9_armhf_v1.3.0.deb`: Pacote de driver para Elgin i9.

---

## Comandos  uteis

- **Listar impressoras configuradas:**
  ```bash
  lpstat -p
  ```

- **Testar a impressão:**
  ```bash
  echo "Teste de impressão" | lp -d <NOME_DA_IMPRESSORA>
  ```

- **Verificar opções disponíveis para uma impressora:**
  ```bash
  lpoptions -p <NOME_DA_IMPRESSORA> -l
  ```

---

Pronto! Suas impressoras Epson TM-20 e Elgin i9 estão configuradas e prontas para uso no Raspberry Pi com CUPS. Se encontrar problemas, abra uma issue neste repositório!

