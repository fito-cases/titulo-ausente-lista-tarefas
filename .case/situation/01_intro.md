# Ticket: cadastro de tarefas sem orientação suficiente

**Contexto:** nossa lista de tarefas funciona no navegador, em uma página HTML. A equipe utiliza cadastro, consulta, edição e exclusão de tarefas; os dados persistem no navegador.

**Relato:** ao tentar cadastrar uma tarefa sem título, o cadastro é rejeitado. A interface não deixa suficientemente claro que o título é obrigatório nem orienta bem quem recebeu a rejeição. Descrição e data devem continuar opcionais.

**Solicitação:** avalie o fluxo pela heurística de reconhecimento, diagnóstico e recuperação de erros. Queremos que uma pessoa consiga entender o que aconteceu e retomar o cadastro; também queremos reduzir as tentativas inválidas previsíveis. Preserve o restante do funcionamento da lista.
