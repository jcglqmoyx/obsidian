```py
bots = ['boa', 'cheetah', 'chicken', 'cobra', 'cockroach', 'cougar', 'goose', 'mantis', 'penguin', 'puma', 'python',
        'raven', 'scarab', 'Scorpion', 'Tiger', 'Widow']

file = open('my.cfg', 'w')
for bot in bots:
    file.write('addbot %s 5 blue 50 %s\n' % (bot, bot))
    file.write('addbot %s 5 red 50 %s\n' % (bot, bot))
file.close()
```
把生成的 `my.cfg` 文件放到 q3ut4 文件夹中。启动游戏后，按下 \` 键（Tab上面的键）， 输入命令 `\bot_enable 1`, 然后输入命令 `\reload`, 游戏会重新加载，加载完成后，按下 \` 键,  输入命令 `\exec my.cfg` 即可。

[Reference](https://www.urbanterror.info/support/188-game-bots/)