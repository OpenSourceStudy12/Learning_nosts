#  function
```
#<include>
/* 获取 termios 信息*/
int tcgetattr(int fd, struct termios *termios_D);
/* set termios 信息 */
int tcsetattr(int fd, int optional_actions, const struct termios *termios_D);

int tcsendbreak(int fd, int duration);

int tcdrain(int fd);
/* 刷新 队列 */
int tcflush(int fd, int queue_selector);

int tcflow(int fd, int acction);

void cfmakeraw(struct termios *termios_D);
/* get input baudrate */
speet_t cfgetispeed(const struct termios *termios_D);
/* get input baudrate */
speet_t cfgetospeed(const struct termios *termios_D);
/* set input speed */
int cfsetispeed(struct termios *termios_D, speet_t speed);
/* set output speed */
int cfsetospeed(struct termios *termios_D, speet_t speed);
/* set in/out speed */
int cfsetspeed(struct termios *termios_D, speet_t speed);
```
# struct termios
```
struct termios {
    tcflag_t c_iflag, /* input modes */
    tcflag_t c_oflag, /* output modes */
    tcfalg_t c_cflag, /* control modes */
    tcflag_t c_lflag, /* local modes */
    cc_t c_cc[NCCS]   /* special characters */
}
```

```
c_iflag:
IGNBRK : 忽略 BREAK 输入
```