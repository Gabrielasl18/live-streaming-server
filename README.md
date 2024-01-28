# live-streaming-server

- Repositório criado para o entendimento básico da construção de um servidor de transmissão ao vivo com NGINX

* Live Streaming

Consumir o conteúdo enquanto ele é baixado ou transmissão de vídeo ao vivo pela internet

---

fluxo

Ingest -> Segmenter <- CDN <- Player

1. Ingest -> encoder

2. Segmenter -> Packager | Storage (disk/DB)

3. CDN

4. Player

---

1. Ingest

> INGERIR é o processo de enviá-lo para um data center ou servidor onde pode ser armazenado e acessado posteriormente. É o primeiro passo em qualquer fluxo de trabalho de produção de vídeo e envolve colocar seus vídeos em seu software em edição ou sistema de armazenamento em nuvem. O arquivo que CHEGA é um vídeo, com a melhor qualidade possível, sem qualquer manipulação.Geralmente o protocolo ingestado não é um protocolo de tranmissão adaptativa. Para o propósito de adaptação dos protocolos ABR ser cumprido, a transmissão precisa ter várias qualidades de vídeo para o mesmo conteúdo. As múltiplas qualidades do vídeo ingestado são geradas por um software chamado de ENCODER, tendo como resultado um protocolo adaptativo ou não.

INGESTÃO é o processo de obter seus arquivos de vídeo em um formato que pode ser usado por outros programas. Os tipos mais comuns de ingestão são: codificação, transcosificação e ingestão através de um clipe mestre.

- Codificação refere-se ao processo de conversão de conteúdo de vídeo em formatos de arquivo de vídeo codificados (por exemplo, ProRes ou H264)
- Transcodificação refere-se a pegar um tipo de arquivo de vídeo codificado e transformá-lo em outro (por exemplo, do Apple ProRes 4444 para o Apple ProRes 422)

Pode ingerir através de uma câmera, ou uma live do computador que está rolando
Utiliza o protocolo RTMP (de entrada de vídeo)

No exemplo do repositório usamos o OBS:

o OBS vai gerar o conteúdo encodado, ou seja, um mp4, um container de vídeo, sendo o vídeo, metadados, áudios, tamanho, formatos, legendas, resolução
esse encoding sao usados para fazer compressao do vídeo no formato que seja trafegado para o dispositivo que vai tocar ele

a fonte pode ser diferente mas vai gerar sempre um mp4

OBS: METADADOS -> Data, formato de arquivo, local, nome, criador/autor, palavras-chave e informações técnicas, como tamanho e tipo de arquivo

---

2. Segmenter

> Está o PACKAGER onde o vídeo é tratado e transformado em diferentes segmentos, numa playlist de protocolo RTMP, ABRTC, SRT, HLS, DASH, MSS, aplicar regras de negócios como criptografia
 - Pega o vídeo e aplica diferentes formatos, resoluções, segmentos, quebra em playlist e protocolos, aplica regras de negócio, faz manipulação da playlist pra inserir anúncios

Como é feito a entrega da playlist?

- Com ADS (Adaptive Bitrate Streaming) com diferentes resoluções pro cliente o player vai baixar o segmentos (chunks)
- pode ser configurada pra entregar e receber com diferentes protocolos de vídeo

- RTMP (Real Time Message Protocol) -> Protocolo desenvolvido sobre TCP para streaming pela internet, voltado para Flash Player (que morreu); Atualmente é muito utilizado para ingest de streaming

- HLS (HTTP Live Streaming Protocol) -> É um protocolo de entrega de conteúdo de áudio e vídeo por HTTP desenvolvido pela apple, vem no formato de tags

tudo que tem .m3u8 é HLS

- DASH (HTTP Live Streaming Protocol) -> É parecido com HLS, Open Source, vem no formato XML

> Também está o STORAGE
 - Armazena os segmentos do vídeo, possibilitando acesso rápido e eficiente.
 - Pode incluir o uso de sistemas de armazenamento em disco ou bancos de dados para manter os segmentos de vídeo

---

4. Delivery

> CDN -> entregar o conteúdo na camada mais próxima do usuário, sem ter a latência (o atraso, desde o tempo real que aconteceu até o momento que vc vê)

Latência | cache | métricas

Tempo de entrega Já ter o conteúdo em memória Entender o comportamento do usuário e vídeo pela plataforma
para o próximo usuário

---

5. Player

> Lado do cliente, o celular, computador etc, navegador web vai ser responsável por decodificar, vai ter que ser capaz de aplicar os algoritmos de decoding, vai ter que lidar com cache, internet ruim

O aplicativo não é um player, o player é dentro da etapa de plataforma e o app que estamos vendo é o produto que utliza o player de uma plataforma de vídeo

O que o player precisar ter?

- Design e customização de estilo
- Thumbnails e prévias
- Acessibilidade
- Legendas e idiomas
- Lidar com anúncios
- ABR
- DRM
- Múltiplos áudios

---

O NGINX é um servidor web de código aberto conhecido por sua eficiência e escalabilidade. Embora não seja diretamente relacionado à reprodução de vídeos, o NGINX é frequentemente utilizado em arquiteturas de streaming de vídeo para melhorar o desempenho e a distribuição de conteúdo. Aqui estão algumas maneiras como o NGINX se encaixa no contexto de um sistema de entrega de vídeo:

Servidor Web e Proxy Reverso:

O NGINX atua como um servidor web eficiente, lidando com solicitações HTTP/HTTPS. Ele pode ser configurado como um proxy reverso para distribuir solicitações entre servidores backend. Isso é útil para balanceamento de carga e gerenciamento de tráfego.
Streaming de Vídeo:

O NGINX pode ser configurado para suportar streaming de vídeo utilizando protocolos como HTTP Live Streaming (HLS), Dynamic Adaptive Streaming over HTTP (DASH), e Smooth Streaming. Ele é capaz de entregar segmentos de vídeo de forma eficiente para os clientes, ajudando na transmissão de conteúdo multimídia.
CDN (Content Delivery Network):

Muitas vezes, o NGINX é utilizado em servidores de borda em uma CDN para otimizar a entrega de conteúdo estático, incluindo vídeos. Ele pode acelerar a distribuição de conteúdo, reduzindo a latência e melhorando a experiência do usuário final.
Cache de Conteúdo:

O NGINX possui recursos de cache que podem ser configurados para armazenar em cache segmentos de vídeo ou até mesmo conteúdo inteiro. Isso reduz a carga nos servidores backend e acelera a entrega de conteúdo para clientes que solicitam o mesmo vídeo repetidamente.
SSL/TLS Termination:

O NGINX pode ser configurado para lidar com a terminação SSL/TLS, aliviando a carga dos servidores de aplicativos. Isso é especialmente útil ao fornecer streaming de vídeo seguro (HTTPS).
Proteção contra DDoS:

NGINX pode ser configurado para ajudar na mitigação de ataques DDoS, protegendo os servidores contra tráfego malicioso e mantendo a disponibilidade do serviço.
Proxy de Aplicações:

Além do streaming de vídeo, o NGINX também pode ser utilizado como um proxy para outras aplicações, como servidores de aplicativos que gerenciam a lógica de negócios relacionada ao sistema de entrega de vídeo.
Em resumo, o NGINX desempenha um papel significativo em arquiteturas de entrega de vídeo, proporcionando eficiência, escalabilidade e otimização de desempenho. É comumente empregado em conjunto com outras ferramentas e plataformas para construir sistemas robustos de streaming de vídeo.
