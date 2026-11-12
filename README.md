# JARVIS AI — Android (Java)

Aplicativo Android em Java com interface futurista, reconhecimento de voz em português, chat por IA e fala via Fish Audio.

## Requisitos
- Android Studio recente
- JDK 17
- Node.js 20+
- Chaves próprias da OpenAI API e Fish Audio

## 1. Configure o backend
Abra o terminal na pasta `backend`:

```bash
npm install
```

Copie `.env.example` para `.env` e preencha `OPENAI_API_KEY` e `FISH_API_KEY`.
Depois execute:

```bash
npm start
```

Teste `http://localhost:3000/health`.

O backend chama a OpenAI Responses API e o Fish Audio TTS. As chaves ficam apenas no servidor, nunca no APK.

## 2. Configure o Android
Abra a pasta raiz deste projeto no Android Studio e aguarde a sincronização do Gradle.

No arquivo `app/src/main/java/com/jarvis/assistant/MainActivity.java`, ajuste `SERVER_URL`:
- Emulador Android: `http://10.0.2.2:3000`
- Celular físico: `http://IP_DO_SEU_COMPUTADOR:3000` (celular e computador na mesma rede)
- Servidor publicado: use o endereço HTTPS do seu backend.

Para celular físico, permita a conexão na rede/firewall do computador. Para produção, publique o backend com HTTPS e altere `SERVER_URL` para HTTPS. O `usesCleartextTraffic` está habilitado apenas para facilitar testes locais.

Compile e execute pelo Android Studio. Na primeira utilização, permita acesso ao microfone.

## 3. Chaves e segurança
- Não coloque chaves no código Android, APK, Git ou prints.
- O arquivo `.env` não deve ser enviado ao repositório.
- A chave Fish Audio que foi compartilhada na conversa deve ser revogada no painel Fish Audio e substituída por uma nova.
- Configure limites de uso e autenticação no backend antes de disponibilizá-lo publicamente. Este exemplo é um ponto de partida e não inclui login/rate limiting.

## Recursos incluídos
- Interface escura com detalhes ciano.
- Envio de mensagens por texto.
- Reconhecimento de fala `pt-BR` pelo serviço de voz do Android.
- Resposta textual via OpenAI Responses API.
- Geração e reprodução de MP3 via Fish Audio.
- Indicadores de estado e histórico durante a sessão.

## Observações
- O reconhecimento de fala usa o serviço de reconhecimento instalado no Android e pode depender da internet/serviços do aparelho.
- O modelo de IA configurado em `OPENAI_MODEL` deve estar habilitado na sua conta e pode gerar custos.
- O modelo Fish Audio `s2.1-pro-free` segue o exemplo fornecido na documentação Fish Audio; disponibilidade e limites dependem da conta.
- Agenda, lembretes, pesquisa web e automações reais ainda precisam de integrações específicas; não são simuladas como se já estivessem funcionando.


## Gerar o APK sem instalar o Android Studio no celular
Este projeto inclui uma automação do GitHub Actions em `.github/workflows/android-apk.yml`.

1. Crie um repositório no GitHub e envie os arquivos desta pasta (não envie seu `.env` nem chaves).
2. Abra a aba **Actions** do repositório.
3. Execute **Build JARVIS APK** (ou aguarde o build ao enviar para `main`).
4. Quando terminar, abra a execução concluída e baixe o artefato **JARVIS-debug-APK**.
5. Extraia o ZIP do artefato e toque em `app-debug.apk` no Android para instalar. Talvez seja necessário permitir instalação para o navegador/gerenciador de arquivos.

O APK gerado por esse fluxo é de depuração (debug), adequado para testes pessoais; não é uma versão publicada/assinada para distribuição na Play Store.

**Atenção:** o APK só conversa com a IA depois que o backend Node.js estiver publicado e `SERVER_URL` em `MainActivity.java` apontar para o endereço HTTPS dele. O backend não fica hospedado dentro do APK.
