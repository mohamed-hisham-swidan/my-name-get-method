# get-script-name-without-libraries

so i was learning `if __name__ == '__main__'` for the first time and i noticed something weird — you can't actually get the name of your main script from inside it, because Python sets `__name__` to `'__main__'` instead of the real filename.

so i thought, what if i import the main script from another file? when Python imports it the second time, it loads it fresh and this time `__name__` is the actual script name.

that's literally it. no `os`, no `sys`, no libraries at all.

## how it works

**script1.py** (your main file)
```python
def funn():
    if __name__ == '__main__':
        i = None
    else:
        return __name__

funn()

def main():
    from script2 import my_real_name
    name = my_real_name()
    print(f"this project's real name is {name}")

main()
```

**script2.py**
```python
def my_real_name():
    from script1 import funn
    name = funn()
    return name
```

run `script1.py` and you get:
```
this project's real name is script1
```

## why not just use os.path.basename(__file__)?

you can, but then you get the full path like `C:\Users\...\script1.py` and you have to clean it up yourself. this gives you the clean name directly with zero imports.

## notes

- yes it prints twice sometimes, that's because of how Python handles the circular import (script1 gets executed again when script2 imports it)
- discovered this while learning Python, figured i'd share it
