# 💾 Working with Amazon EBS: Volume Management, Snapshot Backup & Data Recovery

> ℹ️ **NOTE:** Este é o repositório desenvolvido por **Ismael Santos de Medeiros**.

Projeto prático com o objetivo de demonstrar o gerenciamento de armazenamento em bloco de alta performance utilizando o **Amazon EBS (Elastic Block Store)** em instâncias **Amazon EC2**. O projeto abrange a criação e associação de volumes EBS, formatação de sistemas de arquivos no Linux, configuração de pontos de montagem persistentes, simulação de falhas/perda de dados e a implementação de estratégias de backup e recuperação de desastres através de **Snapshots** do Amazon EBS.

---

## 💻 Tecnologias utilizadas no projeto

- [Amazon EBS (Elastic Block Store)](https://aws.amazon.com/ebs/) — Armazenamento em bloco persistente para instâncias EC2 (`gp3`)
- [Amazon EC2](https://aws.amazon.com/ec2/) — Servidor virtual Linux para associação de discos e testes práticos
- [Amazon EBS Snapshots](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-snapshots.html) — Backups pontuais persistentes armazenados no Amazon S3
- [Linux File Systems (ext3 & fstab)](https://www.kernel.org/) — Formatação de volumes em bloco, montagem em diretórios e configuração de tabelas de montagem persistente
- [EC2 Instance Connect](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-connect-methods.html) — Acesso administrativo seguro via terminal no navegador

---

## ✨ Como foi feito ?

- Criado um volume de dados **Amazon EBS** de 1 GiB do tipo `gp3` na mesma Zona de Disponibilidade da instância EC2 (`us-west-2a`).
- Associado o volume criado à instância Linux no identificador de dispositivo `/dev/sdb`.
- Acessado o terminal via **EC2 Instance Connect** e verificado o estado inicial dos discos com `df -h`.
- Formatado o dispositivo em bloco com o sistema de arquivos `ext3` utilizando o comando `sudo mkfs -t ext3 /dev/sdb`.
- Criado o diretório `/mnt/data-store` e montado o volume EBS configurando o arquivo `/etc/fstab` para garantir a persistência da montagem no reboot.
- Gerado um arquivo de teste (`file.txt`) no volume montado com conteúdo de verificação.
- Criado um **EBS Snapshot** (`My Snapshot`) no console AWS para capturar o estado atual e fazer backup do volume no Amazon S3.
- Simulado um cenário de desastre apagando intencionalmente o arquivo de teste original (`sudo rm`).
- Restaurado o backup criando um novo volume EBS (`Restored Volume`) a partir do snapshot selecionado.
- Anexado o volume restaurado à mesma instância no identificador `/dev/sdc`.
- Criado o ponto de montagem `/mnt/data-store2`, montado o disco restaurado e validada a recuperação completa dos dados originais.

---

## 📚 Materiais

- Documentação do Amazon EBS: [Amazon EBS User Guide](https://docs.aws.amazon.com/ebs/latest/userguide/what-is-ebs.html)
- Guia de Snapshots: [Amazon EBS Snapshots Guide](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-snapshots.html)
- Guia de Montagem Linux: [Make an EBS volume available for use on Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)

---

## 🛠️ Instruções de execução

Para replicar os procedimentos de gerenciamento e backup do Amazon EBS, siga as etapas detalhadas abaixo.

- 🤖 1. No console EC2, acesse a seção **Volumes** e configure a criação de um volume EBS `gp3` de 1 GiB na Zona de Disponibilidade `us-west-2a`.

![Criar Volume EBS](Projetct%20\(2\).png)

- 🤖 2. Confirme a criação do volume `My Volume` exibido no painel de volumes.

![Confirmação da Criação do Volume](Projetct%20\(1\).png)

- 🤖 3. Associe o volume criado à instância `Lab` definindo o nome de dispositivo `/dev/sdb`.

![Associação do Volume à Instância EC2](Projetct%20\(5\).png)

- 🤖 4. Valide no console a confirmação do anexo do volume EBS à instância.

![Confirmação do Anexo do Volume](Projetct%20\(6\).png)

- 🤖 5. Acesse o terminal da instância via EC2 Instance Connect e execute `df -h` para verificar os discos iniciais.

![Verificação Inicial dos Discos](Projetct%20\(9\).png)

- 🤖 6. Formate o novo volume EBS criando um sistema de arquivos `ext3` com o comando `sudo mkfs -t ext3 /dev/sdb`.

![Formatação do Volume EBS](Projetct%20\(10\).png)

- 🤖 7. Crie o diretório de destino `/mnt/data-store` para o ponto de montagem.

![Criação do Diretório de Montagem](Projetct%20\(11\).png)

- 🤖 8. Monte o volume em `/mnt/data-store` e adicione a entrada correspondente no arquivo `/etc/fstab` para garantir a montagem automática na reinicialização.

![Montagem do Disco e Configuração do fstab](Projetct%20\(13\).png)

- 🤖 9. Execute `df -h` para confirmar a correta montagem e disponibilidade do volume `/dev/nvme1n1`.

![Validação da Montagem com df -h](Projetct%20\(14\).png)

- 🤖 10. Crie o arquivo de testes `file.txt` contendo dados de validação no ponto de montagem.

![Escrita do Arquivo de Teste](Projetct%20\(15\).png)

- 🤖 11. No console AWS, acesse a opção de criar snapshot do volume e adicione a tag `Name = My Snapshot`.

![Configuração de Snapshot](Projetct%20\(18\).png)

- 🤖 12. Valide no console a conclusão da solicitação do snapshot.

![Confirmação do Snapshot no Console](Projetct%20\(17\).png)

- 🤖 13. Retorne ao terminal e exclua intencionalmente o arquivo `file.txt` para simular uma perda acidental de dados.

![Exclusão do Arquivo de Teste](Projetct%20\(19\).png)

- 🤖 14. No painel de Snapshots, selecione a opção **Criar volume com o snapshot** no menu de ações.

![Restauração a partir de Snapshot](Projetct%20\(20\).png)

- 🤖 15. Configure a criação do novo volume a partir do snapshot atribuindo o nome `Restored Volume`.

![Criação do Volume Restaurado](Projetct%20\(21\).png)

- 🤖 16. Associe o volume restaurado à instância EC2 utilizando o dispositivo `/dev/sdc`.

![Associação do Volume Restaurado](Projetct%20\(22\).png)

- 🤖 17. Confirme no console da AWS o anexo do novo volume restaurado à instância.

![Confirmação da Associação do Volume Restaurado](Projetct%20\(23\).png)

- 🤖 18. No terminal, crie o ponto `/mnt/data-store2`, monte o volume `/dev/sdc` e confirme a recuperação bem-sucedida do arquivo e seus dados originais.

![Validação da Recuperação de Dados](Projetct%20\(27\).png)

> **📹 Demonstração em Vídeo:** Assista ao passo a passo completo da execução do laboratório acelerado no YouTube:

[![Assistir Demonstração no YouTube](https://img.shields.io/badge/▶️_Assistir_Demonstração_Completa-YouTube-red?style=for-the-badge&logo=youtube)](https://youtu.be/G5l6nP1xcrE)

---

## 👨‍💻 Expert

<p>
    <img 
      align=left 
      margin=10 
      width=80 
      src="https://avatars.githubusercontent.com/u/105826184?v=4"
    />
    <p>&nbsp&nbsp&nbspIsmael Medeiros<br>
    &nbsp&nbsp&nbsp
    <a 
        href="https://github.com/ism-dev-codes">
        GitHub
    </a>
    &nbsp;|&nbsp;
    <a 
        href="https://www.linkedin.com/in/ismael-medeiros">
        LinkedIn
    </a>
    &nbsp;|&nbsp;
    <a 
        href="https://www.instagram.com/ismaelsmedeiros?igsh=YXA1OW1mNXhkNmVy">
        Instagram
    </a>
    &nbsp;|&nbsp;</p>
</p>
<br/><br/>
<p>
