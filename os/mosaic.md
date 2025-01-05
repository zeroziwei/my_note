---
tags: 
progress: 
creation date: 2024-10-05 22:55
modification date: 星期六 5日 十月 2024 22:55:30
---
def main(): 2 pid = sys_fork() 3 sys_sched() # non-deterministic context switch 4 if pid == 0: 5 sys_write('World\n') 6 else: 7 sys_write('Hello\n') 8 9# Outputs: 10# Hello\nWorld 11# World\nHello