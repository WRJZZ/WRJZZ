# 游戏项目——拼图小游戏

## 主函数

```java

package Puzzle_Game;

import java.util.ArrayList;
import Puzzle_Game.Screem.LoginJFrame;
import Puzzle_Game.Screem.User;

public class APP {
    static ArrayList<User> list = new ArrayList<>();
    static {
        list.add(new User("000Jzz", "123123"));
        list.add(new User("001Zyx", "123123"));
    }

    public static void main(String[] args) {

        new LoginJFrame(list);

    }
}

```

## 游戏界面

```java

package Puzzle_Game.Screem;

import java.util.ArrayList;
import java.util.Random;

import javax.swing.ImageIcon;
import javax.swing.JDialog;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;
import javax.swing.WindowConstants;
import javax.swing.border.BevelBorder;
import java.awt.event.KeyListener;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;

public class GameJFrame extends JFrame implements KeyListener, ActionListener {
    // 创建选项下面的条目对象
    JMenuItem replayItem = new JMenuItem("重新开始");
    JMenuItem reLoginItem = new JMenuItem("重新登录");
    JMenuItem closeItem = new JMenuItem("关闭游戏");

    JMenuItem accountItem = new JMenuItem("公众号");
    // 计数
    int step = 0;
    // 3.创建一个二维数组(用来管理数据，加载图片的时候用二维数组的数据来加载)
    int[][] date = new int[4][4];

    // 记录白色方块在二位数组中的位置
    int x = 0;
    int y = 0;

    // 定义一个变量，记录当前展示图片的路径
    String path = "Puzzle_Game\\picture\\girl1\\";

    // 定义一个二维数组，存储正确的数据
    int win[][] = {
            { 2, 3, 4, 5 },
            { 6, 7, 8, 9 },
            { 10, 11, 12, 13 },
            { 14, 15, 16, 1 }
    };
    ArrayList<User> list = LoginJFrame.list;

    // 此界面用于写与游戏界面有关的代码
    public GameJFrame() {
        // 初始化界面
        initJMenu();

        // 初始化菜单
        initJMenuBar();

        // 初始化数据（打乱）
        initDate();

        // 初始化图片（根据打乱的结果去加载图片）
        initImage();

        // 让菜单显示出来
        this.setVisible(true);

    }

    private void initDate() {
        // 1. 定义一个一维数组
        int[] arr = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16 };
        // 2. 打乱数组小中数据的顺序
        Random r = new Random();
        for (int i = 0; i < arr.length; i++) {
            int index = r.nextInt(arr.length);
            int temp = arr[i];
            arr[i] = arr[index];
            arr[index] = temp;
        }

        // 4. 给二维数组添加数据
        // 解法一：
        /*
         * for (int i = 0; i < 9; i++) {
         * date[i / 3][i % 3] = arr[i];
         * }
         */
        // 解法二：
        int temp = 0;
        for (int i = 0; i < date.length; i++) {
            for (int j = 0; j < date[i].length; j++) {
                if (arr[temp] == 1) {
                    x = i;
                    y = j;
                }
                date[i][j] = arr[temp];
                temp++;
            }
        }
    }

    // 初始化图片
    private void initImage() {

        // 清空原本已经出现的图片
        this.getContentPane().removeAll();

        // 细节：
        // 先加载的图片放上面，后加载的放下面
        if (victory()) {
            JLabel winJLabel = new JLabel(new ImageIcon("Puzzle_Game\\picture\\win.jpg"));
            winJLabel.setBounds(93, 273, 480, 480);
            this.getContentPane().add(winJLabel);
        }

        // 记录步数
        JLabel setstep = new JLabel("步数" + step);
        setstep.setBounds(0, 0, 40, 40);
        this.getContentPane().add(setstep);

        for (int i = 0; i < 4; i++) {
            for (int j = 0; j < 4; j++) {
                // 获取当前加载图片的序号
                int num = date[i][j];
                // 创建一个 JLabel 的对象 // 创建一个图片ImageIcon的对象
                JLabel jLabel = new JLabel(
                        new ImageIcon(path + num + ".jpg"));//Puzzle_Game\picture\girl1\7.jpg
                // 指定图片位置
                jLabel.setBounds(120 * j + 93, 120 * i + 273, 120, 120);
                // 给图片添加边框
                jLabel.setBorder(new BevelBorder(0));
                // 把管理容器添加到界面
                // this.add(jLabel);
                this.getContentPane().add(jLabel);
            }
        }

        // 添加背景图片
        JLabel background = new JLabel(new ImageIcon("Puzzle_Game\\picture\\background.png"));
        background.setBounds(25, 25, 620, 760);

        // 把背景图片添加到界面当中
        this.getContentPane().add(background);

        // 刷新界面
        this.getContentPane().repaint();
        // // 创建一个图片ImageIcon的对象
        // ImageIcon icon = new ImageIcon("D:\\Java_code\\Puzzle_Game\\picture\\1.jpg");
        // // 创建一个 JLabel 的对象
        // JLabel jLabel = new JLabel(icon);
        // // 指定图片位置
        // jLabel.setBounds(50, 0, 480, 480)f;

        // // 把管理容器添加到界面
        // // this.add(jLabel);
        // this.getContentPane().add(jLabel);
    }

    private void initJMenuBar() {

        // 创建整个的菜单对象
        JMenuBar jMenuBar = new JMenuBar();

        // 创建菜单上面的两个选项的对象（功能，关于我们）
        JMenu functionJmenu = new JMenu("功能");
        JMenu aboutJMenu = new JMenu("关于我们");

        // 将每一个选项下面的条目添加到选项当中
        functionJmenu.add(replayItem);
        functionJmenu.add(reLoginItem);
        functionJmenu.add(closeItem);

        aboutJMenu.add(accountItem);

        // 给条目绑定事件
        replayItem.addActionListener(this);
        reLoginItem.addActionListener(this);
        closeItem.addActionListener(this);
        accountItem.addActionListener(this);

        // 将菜单里面的两个选项添加到菜单当中
        jMenuBar.add(functionJmenu);
        jMenuBar.add(aboutJMenu);

        // 给整个页面设置菜单
        this.setJMenuBar(jMenuBar);
    }

    private void initJMenu() {
        // 设置界面的宽高
        this.setSize(680, 900);
        // 设置界面的标题
        this.setTitle("拼图单机小游戏 v1.0");
        // 设置界面置顶
        this.setAlwaysOnTop(true);
        // 设置界面居中
        this.setLocationRelativeTo(null);
        // 设置关闭模式
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        // 取消默认的居中放置,只有取消了才会按照XY轴的形式添加组件
        this.setLayout(null);
        // 给整个界面添加键盘监听事件
        this.addKeyListener(this);
    }

    @Override
    public void keyTyped(KeyEvent e) {

    }

    @Override
    public void keyPressed(KeyEvent e) {
        int code = e.getKeyCode();
        // 按住A不松，显示完整图片
        if (code == 65) {
            System.out.println("显示完整图片");
            // 清空原本已经出现的图片
            this.getContentPane().removeAll();
            JLabel jLabel = new JLabel(new ImageIcon("Puzzle_Game\\picture\\girl1\\girl.jpg"));
            jLabel.setBounds(93, 273, 480, 480);
            this.getContentPane().add(jLabel);
            // 添加背景图片
            JLabel background = new JLabel(new ImageIcon("Puzzle_Game\\picture\\background.png"));
            background.setBounds(25, 25, 620, 760);
            // 把背景图片添加到界面当中
            this.getContentPane().add(background);

            // 刷新界面
            this.getContentPane().repaint();

            // 87 -> W
            // 一键通关
        } else if (code == 87) {
            date = new int[][] {
                    { 2, 3, 4, 5 },
                    { 6, 7, 8, 9 },
                    { 10, 11, 12, 13 },
                    { 14, 15, 16, 1 }
            };
            initImage();
        }
    }

    @Override
    public void keyReleased(KeyEvent e) {
        // 判断游戏是否胜利，如果胜利，此方法需要直接结束
        if (victory()) {
            return;
        }
        // 对上下左右进行判断
        int code = e.getKeyCode();
        // System.out.println(code);
        if (code == 37) {
            System.out.println("向左移动");
            if (y == 0) {
                return;
            }
            // x,y表示空白方块
            // x,y-1表示空白方块左边的方块
            date[x][y] = date[x][y - 1];
            date[x][y - 1] = 1;
            y--;
            step++;
            initImage();
        } else if (code == 38) {
            System.out.println("向上移动");
            if (x == 0) {
                return;
            }
            date[x][y] = date[x - 1][y];
            date[x - 1][y] = 1;
            x--;
            step++;
            initImage();
        } else if (code == 39) {
            System.out.println("向右移动");
            if (y == 3) {
                return;
            }
            date[x][y] = date[x][y + 1];
            date[x][y + 1] = 1;
            y++;
            step++;
            initImage();
        } else if (code == 40) {
            System.out.println("向下移动");
            if (x == 3) {
                return;
            }
            date[x][y] = date[x + 1][y];
            date[x + 1][y] = 1;
            x++;
            step++;
            initImage();
        } else if (code == 65) {
            initImage();
        }
    }

    // 胜利
    public boolean victory() {
        for (int i = 0; i < date.length; i++) {
            for (int j = 0; j < date.length; j++) {
                if (date[i][j] != win[i][j]) {
                    return false;
                }
            }
        }
        return true;
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        Object obj = e.getSource();
        if (obj == replayItem) {
            System.out.println("重新游戏");
            step = 0;
            // 再次打乱二维数组中的数据
            initDate();
            // 初始化图片
            initImage();
        } else if (obj == reLoginItem) {
            System.out.println("重新登录");
            this.setVisible(false);
            new LoginJFrame(list);
        } else if (obj == closeItem) {
            System.out.println("关闭游戏");
            System.exit(0);
        } else if (obj == accountItem) {
            System.out.println("公众号");
            // 创建一个公众号
            // 创建一个弹窗对象
            JDialog jDialog = new JDialog();
            // 创建一个管理图片的容器对象jLabel
            JLabel jLabel = new JLabel(new ImageIcon("Puzzle_Game\\picture\\girl1\\girl.jpg"));
            // 设置位置和宽高
            jLabel.setBounds(0, 0, 480, 480);
            // 将图片添加弹窗里
            jDialog.getContentPane().add(jLabel);
            // 给弹窗设置大小
            jDialog.setSize(500, 500);
            // 让弹窗置顶
            jDialog.setAlwaysOnTop(true);
            // 让弹窗居中
            jDialog.setLocationRelativeTo(null);
            // 弹窗不关闭无法操作下面的界面
            jDialog.setModal(true);
            // 让弹窗显示出来
            jDialog.setVisible(true);
        }
    }
}

```

## 登录界面

```java

package Puzzle_Game.Screem;

import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.util.ArrayList;

import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JDialog;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.WindowConstants;
import javax.swing.JTextField;

public class LoginJFrame extends JFrame implements MouseListener {
    static ArrayList<User> list = new ArrayList<>();
    static {
        list.add(new User("000Jzz", "123123"));
        list.add(new User("001Zyx", "123123"));
    }

    JTextField username = new JTextField();
    JTextField usercode = new JTextField();
    JTextField code = new JTextField();
    JButton login = new JButton();
    JButton register = new JButton();
    String codestr;
    JLabel rightcode = new JLabel();
    JButton changecode = new JButton();

    // 此界面用于写与登录界面有关的代码
    public LoginJFrame(ArrayList<User> list) {
        // 初始化界面
        initInterface();
        // 用户登录
        initUser();
        // 显示页面
        this.setVisible(true);
    }

    private void initUser() {

        // 更换验证码
        changecode.setBounds(345, 255, 95, 14);
        changecode.setIcon(new ImageIcon("Puzzle_Game\\picture\\change.jpg"));
        this.getContentPane().add(changecode);
        changecode.addMouseListener(this);

        // 添加用户名文字
        // JLabel usernameText = new JLabel(new ImageIcon());
        // usernameText.setBounds(150, 120, 206, 30);
        // this.getContentPane().add(usernameText);
        // 添加用户名输入框

        username.setBounds(133, 128, 210, 35);
        this.getContentPane().add(username);
        // 添加密码文字
        // JLabel usercodeText = new JLabel(new ImageIcon(""));
        // usercodeText.setBounds(EXIT_ON_CLOSE, ABORT, WIDTH, HEIGHT);
        // this.getContentPane().add(usercodeText);
        // 添加密码输入框

        usercode.setBounds(133, 172, 210, 35);
        this.getContentPane().add(usercode);

        // 验证码提示
        // JLabel codeText = new JLabel(new ImageIcon());
        // codeText.setBounds(150, 216, 20, 20);
        // this.getContentPane().add(codeText);

        // 验证码输入框

        code.setBounds(133, 216, 210, 35);
        this.getContentPane().add(code);

        // 获取验证码
        getRightcode();

        // 添加登录按钮
        login.setBounds(133, 272, 210, 35);
        login.setIcon(new ImageIcon("Puzzle_Game\\picture\\login.jpg"));
        // 去除按钮边框
        login.setBorderPainted(true);
        // 去除按钮背景
        login.setContentAreaFilled(false);
        this.getContentPane().add(login);
        login.addMouseListener(this);

        // 添加注册按钮
        register.setBounds(133, 312, 210, 35);
        register.setIcon(new ImageIcon("Puzzle_Game\\picture\\register.jpg"));
        // 去除按钮边框
        register.setBorderPainted(true);
        // 去除按钮背景
        register.setContentAreaFilled(false);
        this.getContentPane().add(register);

        register.addMouseListener(this);

        // 添加背景图片
        JLabel background = new JLabel(new ImageIcon("D:\\Java_code\\Puzzle_Game\\picture\\background_login.jpg"));
        background.setBounds(0, 0, 480, 430);
        this.getContentPane().add(background);
    }

    private void initInterface() {
        this.setSize(488, 430);
        // 设置界面的标题
        this.setTitle("登录界面");
        // 设置界面置顶
        this.setAlwaysOnTop(true);
        // 设置界面居中
        this.setLocationRelativeTo(null);
        // 设置关闭模式
        this.setDefaultCloseOperation(WindowConstants.DISPOSE_ON_CLOSE);

    }

    @Override
    public void mouseClicked(MouseEvent e) {
        // 单击
        if (e.getSource() == login) {
            // 获取填入的信息
            System.out.println("登录中");
            String idStr = username.getText();
            String passwordStr = usercode.getText();
            String codeStr = code.getText();

            if (idStr.length() == 0) {
                showJDialog("请输入用户名");
                return;
            } else if (passwordStr.length() == 0) {
                showJDialog("请输入密码");
                return;
            } else if (codeStr.length() == 0) {
                showJDialog("请输入验证码");
                return;
            }

            if (codeStr.equals(codestr)) {
                int result = Check(idStr, passwordStr, list);
                if (result == 1) {
                    showJDialog("登录成功");
                    new GameJFrame();
                    this.setVisible(false);

                } else {
                    showJDialog("登录失败");
                    return;
                }
            } else {
                showJDialog("验证码错误");
                codestr = Vcode.V_code();
                rightcode.setText(codestr);
                return;
            }
        }

        else if (e.getSource() == register)

        {
            System.out.println("注册中");
            new RegistJFrame();
        }

        // System.out.println(obj);
    }

    @Override
    public void mousePressed(MouseEvent e) {
        return;
    }

    @Override
    public void mouseReleased(MouseEvent e) {
        if (e.getSource() == changecode) {
            codestr = Vcode.V_code();
            rightcode.setText(codestr);
        }
    }

    @Override
    public void mouseEntered(MouseEvent e) {
        return;
    }

    @Override
    public void mouseExited(MouseEvent e) {
        return;
    }

    // 检验用户名、密码是否正确
    private int Check(String idStr, String passwordStr, ArrayList<User> list) {

        for (int i = 0; i < list.size(); i++) {
            if (idStr.equals(list.get(i).getId())) {
                if (passwordStr.equals(list.get(i).getPassword())) {
                    return 1;
                }
            }
        }
        return 0;
    }

    // 创建一个弹窗
    public void showJDialog(String content) {
        // 创建一个弹框对象
        JDialog jDialog = new JDialog();
        // 给弹框设置大小
        jDialog.setSize(200, 150);
        // 让弹框置顶
        jDialog.setAlwaysOnTop(true);
        // 让弹框居中
        jDialog.setLocationRelativeTo(null);
        // 弹框不关闭永远无法操作下面的界面
        jDialog.setModal(true);

        // 创建Jlabel对象管理文字并添加到弹框当中
        JLabel warning = new JLabel(content);
        warning.setBounds(0, 0, 200, 150);
        jDialog.getContentPane().add(warning);

        // 让弹框展示出来
        jDialog.setVisible(true);
    }

    // 得到正确的验证码
    private void getRightcode() {
        // 生成验证码
        codestr = Vcode.V_code();
        // 设置内容
        rightcode.setText(codestr);
        // 位置和宽高
        rightcode.setBounds(360, 228, 50, 20);
        // 添加到界面
        this.getContentPane().add(rightcode);
    }

}

```

## 注册界面

```java

package Puzzle_Game.Screem;

import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JDialog;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JTextField;
import javax.swing.WindowConstants;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.util.ArrayList;

public class RegistJFrame extends JFrame implements MouseListener {
    static ArrayList<User> list = LoginJFrame.list;
    JTextField username = new JTextField();
    JTextField usercode = new JTextField();
    JTextField code = new JTextField();
    JButton register = new JButton();
    String codestr;
    JLabel rightcode = new JLabel();
    JButton changecode = new JButton();

    // 此界面用于写与注册界面有关的代码
    public RegistJFrame() {
        // 初始化界面
        initInterface();
        // 初始化数据
        initUser();
        // 显示
        this.setVisible(true);
    }

    private void initUser() {
        // 更换验证码

        changecode.setBounds(360, 300, 95, 14);
        changecode.setIcon(new ImageIcon("Puzzle_Game\\picture\\change.jpg"));
        this.getContentPane().add(changecode);
        changecode.addMouseListener(this);

        // 添加用户名文字
        // JLabel usernameText = new JLabel(new ImageIcon());
        // usernameText.setBounds(150, 120, 206, 30);
        // this.getContentPane().add(usernameText);
        // 添加用户名输入框

        username.setBounds(115, 150, 250, 45);
        this.getContentPane().add(username);
        // 添加密码文字
        // JLabel usercodeText = new JLabel(new ImageIcon(""));
        // usercodeText.setBounds(EXIT_ON_CLOSE, ABORT, WIDTH, HEIGHT);
        // this.getContentPane().add(usercodeText);
        // 添加密码输入框

        usercode.setBounds(115, 200, 250, 45);
        this.getContentPane().add(usercode);

        // 验证码提示
        // JLabel codeText = new JLabel(new ImageIcon());
        // codeText.setBounds(150, 216, 20, 20);
        // this.getContentPane().add(codeText);

        // 验证码输入框

        code.setBounds(115, 250, 250, 45);
        this.getContentPane().add(code);

        // 获取验证码
        getRightcode();

        // 添加注册按钮
        register.setBounds(115, 335, 249, 65);
        register.setIcon(new ImageIcon("Puzzle_Game\\picture\\register_now.jpg"));
        // 去除按钮边框
        register.setBorderPainted(true);
        // 去除按钮背景
        register.setContentAreaFilled(false);
        this.getContentPane().add(register);

        register.addMouseListener(this);

        // 添加背景图片
        JLabel background = new JLabel(new ImageIcon("Puzzle_Game\\picture\\background_register.jpg"));
        background.setBounds(0, 0, 488, 500);
        this.getContentPane().add(background);

    }

    private void initInterface() {
        this.setSize(488, 500);
        // 设置界面的标题
        this.setTitle("注册界面");
        // 设置界面置顶
        this.setAlwaysOnTop(true);
        // 设置界面居中
        this.setLocationRelativeTo(null);
        // 设置关闭模式
        this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);

    }

    @Override
    public void mouseClicked(MouseEvent e) {

        if (e.getSource() == register) {
            System.out.println("注册中");
            String idString = username.getText();
            String passwordString = usercode.getText();
            String codeString = code.getText();

            if (idString.length() == 0) {
                showJDialog("请输入用户名");
                return;
            } else if (passwordString.length() == 0) {
                showJDialog("请输入密码");
                return;
            } else if (codeString.length() == 0) {
                showJDialog("请输入验证码");
                return;
            }
            if (codeString.equals(codestr)) {
                System.out.println("注册成功！");
                User u = new User(idString, passwordString);
                list.add(u);
                showJDialog("注册成功");
                new LoginJFrame(list);
                this.setVisible(false);
            } else {
                showJDialog("验证码错误");
                codestr = Vcode.V_code();
                rightcode.setText(codestr);
                return;
            }
        }
    }

    @Override
    public void mousePressed(MouseEvent e) {
        return;
    }

    @Override
    public void mouseReleased(MouseEvent e) {
        if (e.getSource() == changecode) {
            codestr = Vcode.V_code();
            rightcode.setText(codestr);
        }
    }

    @Override
    public void mouseEntered(MouseEvent e) {
        return;
    }

    @Override
    public void mouseExited(MouseEvent e) {
        return;
    }

    // 得到正确的验证码
    private void getRightcode() {
        // 生成验证码
        codestr = Vcode.V_code();
        // 设置内容
        rightcode.setText(codestr);
        // 位置和宽高
        rightcode.setBounds(385, 270, 50, 20);
        // 添加到界面
        this.getContentPane().add(rightcode);
    }

    // 创建一个弹窗
    public void showJDialog(String content) {
        // 创建一个弹框对象
        JDialog jDialog = new JDialog();
        // 给弹框设置大小
        jDialog.setSize(200, 150);
        // 让弹框置顶
        jDialog.setAlwaysOnTop(true);
        // 让弹框居中
        jDialog.setLocationRelativeTo(null);
        // 弹框不关闭永远无法操作下面的界面
        jDialog.setModal(true);

        // 创建Jlabel对象管理文字并添加到弹框当中
        JLabel warning = new JLabel(content);
        warning.setBounds(0, 0, 200, 150);
        jDialog.getContentPane().add(warning);

        // 让弹框展示出来
        jDialog.setVisible(true);
    }
}

```

## 玩家

```java

package Puzzle_Game.Screem;
public class User {
    private String id;// 账号
    private String password;// 密码

    public User(String id, String password) {
        this.id = id;
        this.password = password;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

}

```

## 方法\_获取验证码

```java

package Puzzle_Game.Screem;

import java.util.Random;

public class Vcode {
    public static String V_code() {
        char[] chs = new char[52];
        for (int i = 0; i < chs.length; i++) {
            if (i <= 25) {
                chs[i] = (char) (97 + i);
            } else {
                chs[i] = (char) (65 + i - 26);
            }
        }
        String result = "";
        Random r = new Random();
        for (int i = 0; i < 4; i++) {
            int RandomIndex = r.nextInt(chs.length);
            result = result + chs[RandomIndex];
        }
        int count = r.nextInt(10);
        result = result + count;
        String str = result.toString();
        // Scanner sc =new Scanner(System.in);
        // String str = sc.next();
        // char[] strarr = str.toCharArray();

        char[] strarr = str.toCharArray();
        String v_code = getString(strarr);
        return v_code;
    }

    public static String getString(char[] arr) {
        Random r = new Random();
        int p = r.nextInt(arr.length);
        for (int i = 0; i < arr.length; i++) {
            char temp = arr[i];
            arr[i] = arr[p];
            arr[p] = temp;
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < arr.length; i++) {
            sb.append(arr[i]);
        }
        return sb.toString();
    }

}

```
