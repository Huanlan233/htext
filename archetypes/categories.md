+++
title = "{{ replace .File.ContentBaseName "-" " " | title }}"
description = ""
image = "https://picsum.photos/seed/{{ strings.Substr ( .Date | crypto.SHA256 ) 0 8 }}/250/250"
+++