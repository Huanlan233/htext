+++
title = "{{ replace .File.ContentBaseName "-" " " | title }}"
date = {{ .Date }}
draft = true
categories = []
tags = []
image = "https://picsum.photos/seed/{{ strings.Substr ( .Date | crypto.SHA256 ) 0 8 }}/800/600"
slug = "{{ strings.Substr ( .Date | crypto.SHA256 ) 0 8 }}"
+++
