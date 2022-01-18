# Fix Python Command Launches Microsoft Store in Windows 10
So on the latest version of Windows 10, I faced an issue after installing [Python](https://www.python.org/downloads/) 3.9 64-bit. After installing Python, I added its path to the system PATH variable. But when I tried to run Python from the **cmd.exe** console, it launched Microsoft Store instead.

At first I thought that I had accidentally launched Microsoft Store by mistake. So I tried again, but Microsoft Store was launched once again. Finally, I realized that running the python command is actually launching the Microsoft Store.

Fortunately, fix for this problem is very easy. Windows has linked Python command to initiate installation of Python from the Microsoft store in the hope that it will help users install Python easily. They actually give step-by-step instructions on [installing Python from Microsoft Store](https://docs.microsoft.com/en-us/windows/python/beginners). But it gives a big headache to the rest of the users who directly download it from the official Python website.

Here is how you can fix this problem very easily:

1.  Open Windows Settings by clicking on Start and then on the small cogwheel icon. Alternatively, you can also use the hotkey **Win+I**.

2.  In the Windows Settings, select **Apps** category.![](https://i0.wp.com/www.trishtech.com/wp-content/uploads/2021/08/python-launches-microsoft-store-0.jpg?resize=671%2C535&ssl=1)

3.  Under the Apps category, select **Apps & Features** and then click on **App execution aliases**.\[![](https://i0.wp.com/www.trishtech.com/wp-content/uploads/2021/08/python-launches-microsoft-store-1.jpg?resize=671%2C535&ssl=1)

4.  You will find three app execution aliases for Python related executables – _Python.exe_, _Python3.exe_ and _Python3.7.exe_. Disable them all. \[![](https://i2.wp.com/www.trishtech.com/wp-content/uploads/2021/08/python-launches-microsoft-store-2.jpg?resize=671%2C535&ssl=1)

5.  This is it, you have fixed the problem. Now you can give the **python.exe** command and it will run Python interpreter.\[![](https://i1.wp.com/www.trishtech.com/wp-content/uploads/2021/08/python-launches-microsoft-store-3.jpg?resize=657%2C349&ssl=1)

Microsoft has added these aliases for Python in the good faith that it will help the beginners install Python and other development tools very easily from the Microsoft Store. But if this gives you trouble just because you prefer the latest version of Python downloaded fresh from the [official Python website](https://www.python.org/downloads/), then you can quickly use the above steps to disable these aliases and fix the problem very easily. 
 [https://www.trishtech.com/2021/08/fix-python-command-launches-microsoft-store-in-windows-10/](https://www.trishtech.com/2021/08/fix-python-command-launches-microsoft-store-in-windows-10/)
