## NeuroChat — Android приложение с нейросетями

Приложение для Android с поддержкой множества бесплатных моделей нейросетей через OpenRouter API, генерацией и распознаванием изображений через Hugging Face, с встроенным DNS-over-HTTPS от Comss.one для обхода блокировок.

### Основные возможности

- ✅ **11 бесплатных моделей** для чата с понятными названиями
- ✅ **Генерация изображений** через Hugging Face (5 моделей)
- ✅ **Распознавание изображений** через Hugging Face (6 моделей)
- ✅ **Отправка фото** в чат с нейросетью
- ✅ **Ротация API ключей** — автоматическое переключение при ошибках (14 ключей)
- ✅ **Встроенный DNS-over-HTTPS** от Comss.one для обхода блокировок
- ✅ **Режим множественных запросов** — отправка одного запроса в 5 моделей одновременно
- ✅ **Интерфейс в стиле ChatGPT** с плавными анимациями
- ✅ **Переключение между моделями** в реальном времени
- ✅ **Множественные чаты** с поддержкой истории
- ✅ **Копирование сообщений** в буфер обмена

### Поддерживаемые модели чата

1. **MiniMax M2** (`minimax/minimax-m2:free`)
2. **DeepSeek R1 Chimera** (`tngtech/deepseek-r1t2-chimera:free`)
3. **DeepSeek Chat v3** (`deepseek/deepseek-chat-v3-0324:free`)
4. **DeepSeek R1** (`deepseek/deepseek-r1:free`)
5. **Llama 3.3 70B** (`meta-llama/llama-3.3-70b-instruct:free`)
6. **Nemotron Nano 12B** (`nvidia/nemotron-nano-12b-v2-vl:free`)
7. **GPT OSS 20B** (`openai/gpt-oss-20b:free`)
8. **DeepSeek Chat v3.1** (`deepseek/deepseek-chat-v3.1:free`)
9. **Dolphin Mistral 24B** (`cognitivecomputations/dolphin-mistral-24b-venice-edition:free`)
10. **Tongyi DeepResearch 30B** (`alibaba/tongyi-deepresearch-30b-a3b:free`)
11. **DeepCoder 14B** (`agentica-org/deepcoder-14b-preview:free`)

### Модели генерации изображений

1. **Stable Diffusion XL** (`stabilityai/stable-diffusion-xl-base-1.0`) — высококачественная генерация
2. **Russian Vision v4** (`Daniil-plotnikov/russian-vision-v4`) — русскоязычная модель
3. **Inkpunk Diffusion XL** (`openskyml/inkpunk-diffusion-xl`) — художественный стиль
4. **Anime Release 2** (`peter198477/anime-release2`) — аниме стиль
5. **Soviet Diffusion XL** (`openskyml/soviet-diffusion-xl`) — советский стиль

### Модели распознавания изображений

1. **BLIP Base** (`Salesforce/blip-image-captioning-base`)
2. **ViT-GPT2** (`nlpconnect/vit-gpt2-image-captioning`)
3. **BLIP Large** (`Salesforce/blip-image-captioning-large`)
4. **GIT Base** (`microsoft/git-base`)
5. **GIT Large** (`microsoft/git-large`)
6. **BLIP-2 Base** (`Salesforce/blip-v2-base`)

### DNS-over-HTTPS от Comss.one

Приложение использует встроенный DNS resolver на основе DNS-over-HTTPS от Comss.one:
- **DoH endpoint**: `https://dns.comss.one/dns-query`
- Автоматический fallback на системный DNS при ошибках
- Позволяет обходить блокировки и получать доступ к заблокированным сервисам

### Ротация API ключей

Приложение поддерживает автоматическую ротацию до 14 API ключей OpenRouter:
- При ошибке 429 (rate limit) автоматически переключается на следующий ключ
- При ошибках 403, 401, 404 также происходит переключение
- Все ключи используются равномерно для распределения нагрузки
- Задержка 500мс перед повторной попыткой при rate limit

### Режим множественных запросов

Включите переключатель "5 моделей" для отправки одного запроса параллельно в 5 разных моделей. Вы получите ответы от всех моделей одновременно, что позволяет сравнить их качество и выбрать лучший ответ.

### Требования

- Android Studio Koala+ (или новее)
- JDK 17
- Android SDK 24+ (Android 7.0+)
- Активный интернет
- API ключи OpenRouter (добавьте в `ApiKeyManager.kt`)
- Python сервер для генерации изображений (опционально, см. `IMAGE_GENERATION_README.md`)

### Быстрый старт

1. Откройте проект в Android Studio
2. Добавьте API ключи OpenRouter в `app/src/main/java/com/example/neurochat/ApiKeyManager.kt`
3. Дождитесь синхронизации Gradle
4. Соберите и запустите приложение (Shift+F10 или кнопка Run)

### Сборка APK

**Debug версия:**
```powershell
.\gradlew assembleDebug
```
APK будет в `app/build/outputs/apk/debug/app-debug.apk`

**Release версия:**
```powershell
.\gradlew assembleRelease
```
APK будет в `app/build/outputs/apk/release/app-release.apk`

Подробнее см. `RELEASE_BUILD.md`

### Технологии

- **Kotlin** — основной язык программирования
- **Jetpack Compose** — современный UI фреймворк
- **Material 3** — дизайн система
- **ViewModel + StateFlow** — управление состоянием
- **OkHttp** — HTTP клиент с кастомным DNS resolver
- **kotlinx.serialization** — сериализация JSON
- **OpenRouter API** — единый API для доступа к множеству моделей
- **Hugging Face Inference API** — генерация и распознавание изображений
- **Python Flask** — сервер для генерации изображений (опционально)
- **DNS-over-HTTPS** — безопасный DNS резолвинг

### Структура проекта

```
app/src/main/java/com/example/neurochat/
├── MainActivity.kt              # Главный экран с UI
├── ConversationsViewModel.kt   # Логика чатов и запросов
├── ChatService.kt               # Сервис для работы с API
├── ApiKeyManager.kt             # Ротация API ключей
├── ComssDnsResolver.kt          # DNS-over-HTTPS resolver
├── FreeModels.kt               # Список моделей для чата
├── ImageGenModels.kt            # Модели генерации изображений
├── VisionModels.kt              # Модели распознавания изображений
├── MediaService.kt              # Генерация изображений
├── VisionService.kt             # Распознавание изображений
├── StorageManager.kt            # Сохранение истории чатов
└── ui/theme/
    ├── Theme.kt                 # Тема приложения
    └── Type.kt                  # Типографика
```

### Особенности реализации

1. **Гибридный DNS resolver**: сначала пытается использовать DoH от Comss.one, при ошибке переключается на системный DNS
2. **Параллельные запросы**: использование Kotlin Coroutines для одновременной отправки запросов в несколько моделей
3. **Ротация API ключей**: автоматическое переключение между ключами при ошибках для обхода rate limits
4. **Анимации**: плавные анимации появления сообщений, индикатор печати
5. **Множественные чаты**: поддержка нескольких независимых чатов с переключением между ними
6. **Генерация изображений**: через Python Flask сервер с поддержкой Hugging Face InferenceClient и diffusers
7. **Распознавание изображений**: прямое обращение к Hugging Face Inference API
8. **Оптимизация памяти**: сжатие изображений, ограничение истории чата, обработка в фоновых потоках

### Настройка API ключей

1. Откройте `app/src/main/java/com/example/neurochat/ApiKeyManager.kt`
2. Добавьте ваши API ключи OpenRouter в список `apiKeys`:
```kotlin
private val apiKeys = listOf(
    "sk-or-v1-ваш-ключ-1",
    "sk-or-v1-ваш-ключ-2",
    // ... добавьте до 14 ключей
)
```

### Настройка Python сервера для генерации изображений

Для работы генерации изображений необходимо запустить Python сервер:

1. Установите зависимости:
```bash
pip install flask flask-cors huggingface_hub pillow
pip install torch diffusers transformers accelerate
```

2. Установите переменную окружения `HF_TOKEN` (Hugging Face токен)

3. Запустите сервер:
```bash
python image_generation_server.py
```

Подробнее см. `IMAGE_GENERATION_README.md`

### Безопасность

- API ключи хранятся в коде (для демо)
- Для продакшна рекомендуется использовать бэкенд-прокси
- DNS-over-HTTPS обеспечивает защиту от DNS-спуфинга
- Все запросы идут через HTTPS
- Network Security Config настроен для локальной разработки

### Лицензия

Проект создан для демонстрационных целей.

### Источники

- [OpenRouter Models](https://openrouter.ai/models?fmt=cards&q=free)
- [Hugging Face Models](https://huggingface.co/models)
- [Comss.one DNS](https://www.comss.ru/page.php?id=7315)
