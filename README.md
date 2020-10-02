# AudioSwitch

#### [Версии программы здесь](https://github.com/AsahiOcean/AudioSwitch/releases)

AppleScript для переключения аудиоустройства с помощью [switchaudio-osx](https://github.com/deweller/switchaudio-osx).

Установка [Homebrew](https://brew.sh/)
```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

Устанавливаем switchaudio-osx через homebrew
------------------------
```
$ brew install switchaudio-osx
```

1. Запускаем Automator и создаем программу

![](https://i.ibb.co/yF9Ms0w/1.png)

2. В поиске Automator ищем "AppleScript"

![](https://media.giphy.com/media/4MNWbJwivWcF7rqfAY/giphy.gif)

3. Заменяем код на этот и сохраняем

```
set the current to (do shell script "/usr/local/Cellar/switchaudio-osx/*/SwitchAudioSource -c")

on muteFunc()
	set volume output volume 0
	set volume with output muted
	set volume input volume 0
end muteFunc

muteFunc()
set vol to output volume of (get volume settings)
set mic to input volume of (get volume settings)

if {current, vol, mic} is equal to {"Внешние наушники", 0, 0} then
	display notification "Громкость: output – " & (vol) & "%, input – " & (mic) & "% " with title "Режим без звука 🔕" subtitle "Наушники и микрофон приглушены"
else
	if current is equal to "Динамики «MacBook Pro»" then
		set the devices to (do shell script "/usr/local/Cellar/switchaudio-osx/*/SwitchAudioSource -a")
		if devices contains "Внешние наушники" then
			do shell script "/usr/local/Cellar/switchaudio-osx/*/SwitchAudioSource -s \"Внешние наушники\""
			set the headphones to (do shell script "/usr/local/Cellar/switchaudio-osx/*/SwitchAudioSource -c")
			display notification "Громкость: output – " & (vol) & "%, input – " & (mic) & "%" with title "Переключение на «" & (headphones) & "»" subtitle "" & (headphones) & " приглушены"
		else
			display notification "Выход – " & (vol) & ", вход – " & (mic) & "" with title "Наушники не обнаружены ❌" subtitle "Громкость динамиков и микрофона приглушена"
		end if
	end if
end if
```

4. Добавляем в автозапуск и/или привязываем на горячую клавишу

5. Радуемся жизни 🗿
