```py
bots = ['boa', 'cheetah', 'chicken', 'cobra', 'cockroach', 'cougar', 'goose', 'mantis', 'penguin', 'puma', 'python',
        'raven', 'scarab', 'Scorpion', 'Tiger', 'Widow']

file = open('my.cfg', 'w')
for i in range(0, len(bots), 2):
    file.write('addbot %s 5 blue 50 %s\n' % (bots[i], bots[i]))
    file.write('addbot %s 5 red 50 %s\n' % (bots[i + 1], bots[i + 1]))
file.close()
```
把生成的 `my.cfg` 文件放到 q3ut4 文件夹中。启动游戏后，按下 \` 键（Tab上面的键）， 输入命令 `\bot_enable 1`, 然后输入命令 `\reload`, 游戏会重新加载，加载完成后，按下 \` 键,  输入命令 `\exec my.cfg` 即可。
可以根据想添加的机器人类型、数量等多生成几个不同的 `.cfg` 配置文件，玩游戏时选择想加载的配置文件（用 `\exec` 命令）。

[Reference](https://www.urbanterror.info/support/188-game-bots/)