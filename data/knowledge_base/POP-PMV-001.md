> **DADO SIMULADO — uso acadêmico.** Documento fictício criado pelo grupo. Não substitui procedimento oficial nem as normas NR-10 e NR-35.

# POP-PMV-001 · Procedimento Operacional — PMV (painéis de mensagens variáveis)

Versão: 1.0 (simulada) · Tipo de documento: `pop` · Tipo de ativo: `pmv`

## 0. Pré-requisitos de segurança (obrigatórios antes de qualquer passo)
1. Profissional habilitado: NR-10 para eletricidade e NR-35 para trabalho em altura (pórtico ou painel elevado).
2. EPI: capacete com jugular, calçado de segurança, luvas isolantes quando aplicável, colete de alta visibilidade, cinto paraquedista com trava-quedas.
3. Sinalização de via e comunicação com o CCO; confirmar que não há mensagem crítica ativa (neblina, interdição) antes de desligar ou reiniciar o painel.
4. Desenergizar o painel e o módulo antes de abrir o gabinete (seccionar, impedir a reenergização, constatar ausência de tensão, sinalizar). Nunca abrir o painel energizado.

## 1. Inspeção
1. Confirmar com o CCO o que o painel mostra e o que deveria mostrar.
2. Rodar o padrão de teste de pixels para localizar módulos apagados.
3. Verificar a comunicação com o CCO (último pacote recebido).
4. Inspecionar gabinete, vedação e ventilação.
5. Registrar na OS.

## 2. Preventiva
1. Cumprir o item 0.
2. Teste de todos os módulos de LED (a cada 90 dias).
3. Verificar fontes de alimentação e conexões (a cada 180 dias).
4. Limpar a frente do painel e as aberturas de ventilação.
5. Testar a comunicação com o CCO e a exibição de uma mensagem de teste.

## 3. Corretiva
1. Cumprir o item 0 e classificar o defeito: módulo de LED apagado, mensagem travada ou falha de comunicação.
2. Módulo apagado: verificar a fonte do módulo; substituir a fonte ou o módulo.
3. Mensagem travada: tentar reinício remoto autorizado pelo CCO; se persistir, reiniciar o controlador no local com procedimento de desenergização.
4. Falha de comunicação: verificar o rádio de enlace e a alimentação; substituir o rádio se não houver sinal.
5. Testar com o CCO e registrar a causa e a peça trocada.

## 4. CBM (manutenção por condição)
1. Acompanhar a quantidade de módulos apagados e a reincidência por fonte.
2. Falhas repetidas de módulo de LED (3 ou mais em 12 meses) indicam avaliar troca da fonte ou retrofit.
3. Registrar a decisão na OS e levar à Engenharia de Manutenção.
