# Инструкция по созданию Release версии

## Способ 1: Через командную строку (Gradle)

### Создание Release APK

```bash
# Windows
.\gradlew.bat assembleRelease

# Linux/Mac
./gradlew assembleRelease
```

APK файл будет создан в: `app/build/outputs/apk/release/app-release.apk`

### Создание Release AAB (для Google Play)

```bash
# Windows
.\gradlew.bat bundleRelease

# Linux/Mac
./gradlew bundleRelease
```

AAB файл будет создан в: `app/build/outputs/bundle/release/app-release.aab`

## Способ 2: Через Android Studio

1. Откройте проект в Android Studio
2. В меню выберите: **Build** → **Generate Signed Bundle / APK**
3. Выберите **APK** или **Android App Bundle**
4. Выберите или создайте keystore (см. ниже)
5. Выберите **release** build variant
6. Нажмите **Finish**

## Текущая конфигурация

Сейчас Release версия использует **debug signing** (для тестирования). Это означает:
- ✅ Можно установить на устройство для тестирования
- ✅ Не требует создания keystore
- ❌ Не подходит для публикации в Google Play

## Создание Production Keystore (для публикации)

Для публикации в Google Play нужно создать отдельный keystore:

```bash
keytool -genkey -v -keystore neurochat-release.jks -keyalg RSA -keysize 2048 -validity 10000 -alias neurochat
```

Затем добавьте в `app/build.gradle.kts`:

```kotlin
android {
    signingConfigs {
        create("release") {
            storeFile = file("neurochat-release.jks")
            storePassword = "ваш_пароль"
            keyAlias = "neurochat"
            keyPassword = "ваш_пароль"
        }
    }
    
    buildTypes {
        release {
            signingConfig = signingConfigs.getByName("release")
            isMinifyEnabled = false
            isDebuggable = false
        }
    }
}
```

**⚠️ ВАЖНО:** 
- Сохраните keystore файл и пароли в безопасном месте!
- Без keystore нельзя обновить приложение в Google Play
- Не коммитьте keystore в Git!

## Проверка Release APK

После создания APK можно проверить:

```bash
# Проверить подпись
jarsigner -verify -verbose -certs app/build/outputs/apk/release/app-release.apk

# Проверить размер
dir app\build\outputs\apk\release\app-release.apk
```

## Установка Release APK на устройство

```bash
# Через ADB
adb install app/build/outputs/apk/release/app-release.apk

# Или просто скопируйте APK на устройство и установите вручную
```

## Оптимизация Release версии

Для уменьшения размера APK можно включить:

1. **Minification** (уже отключено):
```kotlin
release {
    isMinifyEnabled = true
    proguardFiles(...)
}
```

2. **Удаление неиспользуемых ресурсов**:
```kotlin
release {
    shrinkResources = true
}
```

3. **Разделение по ABI** (создает отдельные APK для разных архитектур):
```kotlin
splits {
    abi {
        isEnable = true
        reset()
        include("arm64-v8a", "armeabi-v7a", "x86", "x86_64")
    }
}
```

## Текущие настройки Release

- ✅ `isMinifyEnabled = false` - код не минифицируется
- ✅ `isDebuggable = false` - отладка отключена
- ✅ Используется debug signing (для тестирования)
- ✅ ProGuard правила настроены (но не используются)

