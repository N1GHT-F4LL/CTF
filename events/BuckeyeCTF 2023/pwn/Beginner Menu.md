# Description:

I just made this menu for my coding class. I think I covered all the switch cases.
```
nc chall.pwnoh.io 13371
```

# Solution
```
Enter the number of the menu item you want:
1: Hear a joke
2: Tell you the weather
3: Play the number guessing game
4: Quit
```

I saw there was a C file so I used it. Reading through it, I see a function that print_flag() below, and a series of if statements above. So to get the flag, we just need to enter a certain value that makes the if statements above always false, then we will not get exit(0);

if(strcmp(buf, "0\n")==0): This snippet checks whether the string buf is equal to "0\n". If correct, meaning the user entered "0\n", the program will print the message "That's not an option" and exit the program. So 0 is not the value I need.

The atoi() function in C converts a string to a number if possible and returns an int value. If it cannot be converted, it returns 0. So simply we just need to enter anything that cannot be converted into is the number and we will receive the flag

> flag: bctf{y0u_ARe_sNeaKy}
