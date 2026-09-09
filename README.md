# ☁️❄️ Governança de Dados e LGPD no Snowflake Data Cloud 
### 🛡️ Mascaramento Dinâmico, Anonimização Estatística e Controle de Acesso Baseado em Funções (RBAC)

<p align="center">
  <img src="https://img.shields.io/badge/Snowflake-29B5E8?style=for-the-badge&logo=snowflake&logoColor=white" />
  <img src="https://img.shields.io/badge/SQL-Data_Governance-4479A1?style=for-the-badge&logo=sqlite&logoColor=white" />
  <img src="https://img.shields.io/badge/LGPD-Compliance-008000?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Security-Dynamic_Data_Masking-FF0000?style=for-the-badge" />
</p>

---

## 📌 Sobre o Projeto
Este projeto aborda um desafio real e recorrente em arquiteturas de dados modernas na nuvem: **como liberar acesso a dados para análise e BI sem violar diretrizes de privacidade da LGPD (Lei Geral de Proteção de Dados)**.

Utilizando os recursos nativos de governança do **Snowflake Data Cloud**, foi construída uma camada centralizada de segurança que garante:
1. **Mascaramento Dinâmico em Tempo de Execução (Dynamic Data Masking):** Ocultação de PII (Dados Pessoais Identificáveis) como CPF, E-mail e Telefones para perfis não autorizados.
2. **Políticas de Acesso por Linha (Row Access Policies):** Restrição de quais linhas cada cargo ou setor pode visualizar.
3. **Privacidade Diferencial / Ruído Estatístico:** Aplicação de perturbação nos valores de colunas sensíveis (ex: salários) para permitir cálculos agregados (médias, somas) sem expor os valores exatos por indivíduo.

Toda essa lógica é aplicada diretamente no motor do Snowflake, garantindo que as regras permaneçam ativas independentemente da ferramenta consumidora (**Power BI, Python, SQL, Tableau**, etc.).

---

## 📸 
<img width="1834" height="677" alt="Captura de tela de 2026-08-19 13-16-49" src="https://github.com/user-attachments/assets/875656b5-7e80-421a-ae94-7713ce1e94b7" />
<img width="1908" height="925" alt="Captura de tela de 2026-08-18 23-23-32" src="https://github.com/user-attachments/assets/cb741962-37ae-424c-8483-124e4af8dcf4" />


<p align="center">
  <img src="COLE_O_LINK_DA_SUA_IMAGEM_AQUI" alt="Demonstração do Mascaramento no Snowflake" width="100%">
</p>

---

## 📐 Arquitetura de Governança e Fluxo de Dados

```text
  [ Dados Brutos / Sensíveis ] (CPF, E-mail, Salário)
               │
               ▼
   ┌─────────────────────────────────────────────────────────┐
   │              MOTOR DE GOVERNANÇA SNOWFLAKE              │
   ├─────────────────────────────────────────────────────────┤
   │ 🔒 Dynamic Data Masking (Mascara CPF e E-mail em tempo)  │
   │ 🛡️ Row Access Policy (Filtra linhas conforme a Role)   │
   │ 📊 Differential Privacy (Aplica ruído em Salários)      │
   └─────────────────────────────────────────────────────────┘
               │
               ├──────────────────────────┬──────────────────────────┐
               ▼                          ▼                          ▼
     [ Analyst / Power BI ]     [ Auditoria / Compliance ]   [ Admin / DPO ]
     (Dados Mascarados/Agregados)   (Visualização Restrita)     (Acesso Completo)
