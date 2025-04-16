# notes
流水账日记

@echo off
setlocal enabledelayedexpansion

:: 生成所有.xls文件的列表以避免重复处理
dir /s /b *.xls > list.txt

for /f "usebackq delims=" %%F in ("list.txt") do (
    if exist "%%F" (
        set "filepath=%%F"
        set "filename=%%~nF"
        set "char3=!filename:~-3,1!"
		
		if "!char3!" geq "A" if "!char3!" leq "Z" (
			:: 检查最后两个字符是否是草单
			if "!filename:~-2!"=="草单" (
				set "foldername=!filename:~0,-3!"
			) else (
				set "foldername=!filename!"
			)
		) else (
			:: 处理以"草单"结尾的文件名
			if "!filename:~-2!"=="草单" (
				set "foldername=!filename:~0,-2!"
			) else (
				set "foldername=!filename!"
			)
		)
        
        :: 搜索所有同名文件夹并找到最深路径
        set "max_len=0"
        set "target_path="
        
        for /f "delims=" %%A in ('dir /s /b /ad *"!foldername!"* 2^>nul') do (
            set "current_path=%%A"
            call :get_str_length "!current_path!"
            if !str_len! gtr !max_len! (
                set "max_len=!str_len!"
                set "target_path=!current_path!"
            )
        )
        
        :: 移动文件到最深路径
        if defined target_path (
            move "!filepath!" "!target_path!\" >nul
            echo 已移动 "!filepath!" 到 "!target_path!\"
        ) else (
            echo 未找到匹配的文件夹 "!foldername!\"，跳过文件 "!filepath!"
        )
    )
)

del list.txt
pause
exit /b

:get_str_length
setlocal
set "str=%~1"
set "len=0"
:loop
if defined str (
    set /a len+=1
    set "str=!str:~1!"
    goto loop
)
endlocal & set str_len=%len%
goto :eof
