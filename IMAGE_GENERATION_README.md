# Инструкция по настройке генерации изображений

## Требования

1. Python 3.8+
2. Установленные библиотеки:
```bash
# Базовые библиотеки (обязательно)
pip install flask flask-cors huggingface_hub pillow

# Для модели russian-vision-v4 (опционально, если используете эту модель)
pip install diffusers transformers accelerate torch
```

## Настройка

1. Установите переменную окружения `HF_TOKEN` с вашим токеном Hugging Face:
```bash
# Windows (PowerShell)
$env:HF_TOKEN="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"

# Linux/Mac
export HF_TOKEN="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"
```

2. Запустите Python сервер:
```bash
python image_generation_server.py
```

Сервер будет доступен на `http://localhost:5000`

## Настройка Android приложения

### Для эмулятора Android:
- URL по умолчанию: `http://10.0.2.2:5000` (уже настроено)
- Ничего дополнительно настраивать не нужно

### Для реального устройства:
1. Узнайте IP адрес вашего компьютера:
   - Windows: `ipconfig` (IPv4 адрес)
   - Linux/Mac: `ifconfig` или `ip addr`
   
2. Добавьте в `local.properties`:
```
PYTHON_SERVER_URL=http://192.168.1.100:5000
```
(Замените `192.168.1.100` на ваш IP адрес)

3. Убедитесь, что устройство и компьютер в одной сети Wi-Fi

4. Пересоберите приложение

## Доступные модели

- **stabilityai/stable-diffusion-xl-base-1.0** (nscale) - Высококачественная генерация
- **Daniil-plotnikov/russian-vision-v4** (fal-ai) - Русскоязычная модель
- **openskyml/inkpunk-diffusion-xl** (fal-ai) - Художественный стиль
- **peter198477/anime-release2** (fal-ai) - Аниме стиль
- **openskyml/soviet-diffusion-xl** (fal-ai) - Советский стиль

## Проверка работы

1. Проверьте здоровье сервера:
```bash
curl http://localhost:5000/health
```

2. Проверьте список моделей:
```bash
curl http://localhost:5000/models
```

3. Протестируйте генерацию:
```bash
curl -X POST http://localhost:5000/generate \
  -H "Content-Type: application/json" \
  -d '{"prompt": "Astronaut riding a horse", "model_id": "stabilityai/stable-diffusion-xl-base-1.0"}'
```

## Устранение проблем

- **Ошибка подключения**: Убедитесь, что сервер запущен и доступен
- **Timeout**: Увеличьте таймауты в `MediaService.kt` если нужно
- **Ошибка модели**: Проверьте, что модель доступна на Hugging Face и провайдер работает

