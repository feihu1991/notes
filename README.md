# notes
流水账日记

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
