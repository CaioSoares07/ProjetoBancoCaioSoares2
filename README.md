
Projeto de Sistema Bancário Modularizado em Python:

Desenvolvi um sistema bancário completo em Python, utilizando programação modular para criar um código organizado e de fácil manutenção. O projeto inclui funcionalidades essenciais como:

Autenticação de Usuário: Sistema de login seguro com tentativas limitadas e bloqueio de acesso em caso de falha.
Gerenciamento de Contas: Criação de contas correntes vinculadas a usuários, com números sequenciais e agência fixa.
Operações Bancárias: Implementação de funções para depósito, saque (com limites e restrições) e visualização de extrato detalhado.
Cadastro de Clientes: Criação de usuários com informações completas (nome, data de nascimento, CPF, endereço), armazenados em uma lista e com validação de CPF único.
Modularização: Código organizado em funções independentes, seguindo boas práticas de programação e regras de passagem de argumentos específicas.
Habilidades Demonstradas:

Programação em Python
Programação Modular
Estruturas de Dados (listas, dicionários)
Tratamento de Exceções
Desenvolvimento de Sistemas Bancários




usuarios = []
contas = []

def criar_usuario(nome, data_nascimento, cpf, endereco):
  """Cria um novo usuário e o adiciona à lista de usuários."""
  for usuario in usuarios:
    if usuario["cpf"] == cpf:
      print("Erro: CPF já cadastrado.")
      return
  usuarios.append(
      {
          "nome": nome,
          "data_nascimento": data_nascimento,
          "cpf": cpf,
          "endereco": endereco,
      }
  )
  print("Usuário criado com sucesso!")

def criar_conta(usuario):
  """Cria uma nova conta corrente e a associa a um usuário."""
  numero_conta = len(contas) + 1
  conta = {"agencia": "0001", "numero_conta": numero_conta, "usuario": usuario}
  contas.append(conta)
  print("Conta corrente criada com sucesso!")

def saque(*, saldo, extrato, valor, limite, numero_saques):
  """Realiza um saque na conta."""
  excedeu_saldo = valor > saldo
  excedeu_limite = valor > limite
  excedeu_saques = numero_saques >= 3
  if excedeu_saldo:
    print("Operação falhou! Saldo insuficiente.")
  elif excedeu_limite:
    print("Operação falhou! O valor do saque excede o limite.")
  elif excedeu_saques:
    print("Operação falhou! Número máximo de saques excedido.")
  elif valor > 0:
    saldo -= valor
    extrato += f"Saque: R$ {valor:.2f}\n"
    numero_saques += 1
    print("Saque realizado com sucesso!")
  else:
    print("Operação falhou! Valor inválido.")
  return saldo, extrato

def deposito(saldo, valor, extrato, /):
  """Realiza um depósito na conta."""
  if valor > 0:
    saldo += valor
    extrato += f"Depósito: R$ {valor:.2f}\n"
    print("Depósito realizado com sucesso!")
  else:
    print("Operação falhou! Valor inválido.")
  return saldo, extrato

def extrato(saldo, /, *, extrato):
  """Exibe o extrato da conta."""
  print("\n================ EXTRATO ================")
  print("Não foram realizadas movimentações." if not extrato else extrato)
  print(f"\nSaldo: R$ {saldo:.2f}")
  print("==========================================")

senha_correta = "A121314."
tentativas = 3

while tentativas > 0:
  senha = input("Digite a sua senha: ")
  if senha == senha_correta:
    print("Acesso concedido! Bem-vindo ao Banco CaioSoares!.")
    break
  else:
    tentativas -= 1
    print(f"Senha incorreta! Você tem {tentativas} tentativa(s) restante(s).")

if tentativas == 0:
  print("Número máximo de tentativas excedido. Encerrando o programa.")
  exit()

menu = """

[d] Depositar
[s] Sacar
[e] Extrato
[q] Sair

=> """

saldo = 0
limite = 500
extrato_conta = ""
numero_saques = 0

while True:
  opcao = input(menu)

  if opcao == "d":
    valor = float(input("Informe o valor do depósito: "))
    saldo, extrato_conta = deposito(saldo, valor, extrato_conta)

  elif opcao == "s":
    valor = float(input("Informe o valor do saque: "))
    saldo, extrato_conta = saque(
        saldo=saldo,
        extrato=extrato_conta,
        valor=valor,
        limite=limite,
        numero_saques=numero_saques,
    )

  elif opcao == "e":
    extrato(saldo, extrato=extrato_conta)

  elif opcao == "q":
    break

  else:
    print("Operação indisponível, favor selecione novamente a operação desejada."
Validação de Dados
