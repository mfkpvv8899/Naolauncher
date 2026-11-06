# Nôa Launcher (base)

Launcher Android minimalista com wallpaper reativo ao movimento.

## Baixar o APK pelo GitHub (sem Android Studio)
1. Crie um repositório no GitHub e envie todo o conteúdo desta pasta (incluindo `.github/`).
   - Pelo site: **Add file → Upload files**, arraste os arquivos e faça commit na branch `main`.
2. Abra a aba **Actions**. O workflow **Build APK** roda sozinho (leva ~3–6 min).
3. Clique no run concluído → **Artifacts** → baixe **NoaLauncher-apk** (vem em .zip; extraia para obter o `NoaLauncher.apk`).
4. Para um link fixo de download: crie uma **Release** com tag `v0.1.0` (Releases → Draft a new release → Create new tag).
   O APK é anexado automaticamente à Release.
5. No celular: abra o APK e permita "Instalar apps desconhecidos". Depois, Home → Nôa Launcher → "Sempre".

> O APK é **debug** (assinado com a chave de debug do Android): instala normalmente, mas não serve para a Play Store.
> Para um APK release assinado, adicione seu keystore como GitHub Secret e um `signingConfig`.

## Build local
1. Abra a pasta no **Android Studio Koala+** e aguarde o Gradle sync (JDK 17).
2. Rode em um dispositivo/emulador API 26+ (blur real do drawer: API 31+).
3. Pressione Home → escolha **Nôa Launcher** → "Sempre".
4. Long press na tela inicial → Configurações → **Definir wallpaper Nôa Motion**.

## Estrutura
- `wallpaper/MotionWallpaperService.kt` — parallax: acelerômetro → low-pass → lerp → camadas. Sensor só ativo com tela visível.
- `home/HomeActivity.kt` — janela translúcida, edge-to-edge, blur-behind animado, gestos.
- `home/HomeScreen.kt` — relógio, dock translúcido, gestos (swipe up/down, long press, double tap).
- `drawer/AppDrawer.kt` — grade 4 colunas + busca.
- `settings/` — **Tudo transparente** (liga/desliga + nível 0–100%), intensidade do blur, sensibilidade e partículas.
  As mudanças aparecem ao vivo na Home via `NoaPrefs.uiFlow()`.

## Personalizar
- Transparência: `UiPrefs.glass(base)` em `settings/NoaPrefs.kt` define o alpha final de dock, busca, ícones e véu do drawer.
- Sensibilidade/amplitude: `maxShift` em `MotionWallpaperService.draw()`.
- Suavidade: fator do lerp (`0.08f`) e do low-pass (`0.12f`).
- Cores do fundo: gradiente em `onSurfaceChanged`.

## Pendente (próximas etapas)
Hilt, giroscópio, widgets (AppWidgetHost), icon packs, busca de contatos/web, onboarding, tema claro/escuro,
dock configurável, double tap p/ bloquear (AccessibilityService).
