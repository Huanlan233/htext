+++
title = "{{ replace .File.ContentBaseName "-" " " | title }}"
date = {{ .Date }}
draft = true
categories = []
tags = []
slug = "{{ strings.Substr ( .Date | crypto.SHA256 ) 0 8 }}"
+++
