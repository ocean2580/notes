# 开头

参考自尚学堂，==后文代码CV并添加图片资源即可运行==

对代码有一定的重构，功能的添加(按‘G’开挂)

评论区留联系方式（微信除外），看到后会发送图片资源。

# 项目结构

![image-20220405182117921](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220405182117921.png)

TODO

## GamePanel 

   KeyMonitor (内部类) 

   keyPressed(KeyEvent): void

   keyReleased(KeyEvent): void 

launch(): void 

paint(Graphics): void 

main(String[]): void

```java
package com.company.tank;
import javax.swing.*;
import java .awt.*;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class GamePanel extends JFrame {

    /** 定义双缓存图片 */
    private Image offScreenImage = null;
    //游戏状态: 0 游戏未开始，1 单人模式，2 双人模式， 3 游戏暂停， 4 游戏失败，5 游戏成功
    public int state= 0;
    //临时变量
    private int a = 1;
    //重绘次数
    public int count = 0;
    //窗口长宽
    private int width = 800;
    private int height = 610;
    //敌人数量
    private int enemyCount = 0;
    //高度
    private int y = 150;
    //是否开始
    private boolean start = false;
    //物体集合
    public List<Bullet> bulletList = new ArrayList<>();
    public List<Bot> botList = new ArrayList<>();
    public List<Tank> tankList = new ArrayList<>();
    public List<Wall> wallList = new ArrayList<>();
    public List<Bullet> removeList = new ArrayList<>();
    public List<Base> baseList = new ArrayList<>();
    public List<BlastObj> blastList = new ArrayList<>();

    //背景图片
    public Image background = Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/background.jpg"));
    //指针图片
    private Image select = Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/selecttank.gif"));
    //基地
    private Base base = new Base(Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/star.gif"))
, 365, 560, this);


    //玩家
    private PlayerOne playerOne = new PlayerOne(Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/player1/p1tankU.gif")),
             125, 510,
            Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/player1/p1tankU.gif")),Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/player1/p1tankD.gif")),
            Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/player1/p1tankL.gif")),Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/player1/p1tankR.gif")), this);

    private PlayerTwo playerTwo = new PlayerTwo(Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/player2/p2tankU.gif")),
            625, 510,
            Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/player2/p2tankU.gif")),
            Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/player2/p2tankD.gif")),
            Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/player2/p2tankL.gif")),
            Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/player2/p2tankR.gif")), this);

    //窗口的启动方法
    public void launch(){
        //标题
        setTitle("坦克大战");
        //窗口初始大小
        setSize(width, height);
        //用户不能调整大小
        setResizable(false);
        //使窗口可见
        setVisible(true);
        //获取屏幕分辨率，使窗口生成时居中
        setLocationRelativeTo(null);
        //添加关闭事件
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        //添加键盘事件
        this.addKeyListener(new GamePanel.KeyMonitor());
        //添加围墙 60*60
        for(int i = 0; i< 14; i ++){
            wallList.add(new Wall(Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/walls.gif")), i*60 ,170, this ));
        }
        wallList.add(new Wall(Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/walls.gif")), 305 ,560,this ));
        wallList.add(new Wall(Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/walls.gif")), 305 ,500,this ));
        wallList.add(new Wall(Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/walls.gif")), 365 ,500,this ));
        wallList.add(new Wall(Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/walls.gif")), 425 ,500,this ));
        wallList.add(new Wall(Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/walls.gif")), 425 ,560,this ));
        //添加基地
        baseList.add(base);

        while (true){
            if(botList.size() == 0 && enemyCount == 10){
                state = 5;
            }
            if(tankList.size() == 0 && (state == 1 || state == 2)){
                state = 4;
            }
            if(state == 1 || state == 2){
                if (count % 100 == 1 && enemyCount < 10) {
                    Random r = new Random();
                    int rnum =r.nextInt(800);
                    botList.add(new Bot(Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/enemy/enemy1U.gif")), rnum, 110,
                            Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/enemy/enemy1U.gif")),Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/enemy/enemy1D.gif")),
                            Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/enemy/enemy1L.gif")),Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/enemy/enemy1R.gif")), this));
                    enemyCount++;
                }
            }
            repaint();
            try {
                //线程休眠
                Thread.sleep(25);
            }catch (Exception e){
                e.printStackTrace();
            }
        }
    }

    @Override
    public void paint(Graphics g) {
        // 创建和容器一样大小的Image图片
        if(offScreenImage ==null){
            offScreenImage=this.createImage(width, height);
        }
        // 获得该图片的画布
        Graphics gImage= offScreenImage.getGraphics();
        // 背景颜色
        gImage.setColor(Color.gray);
        // 填充整个画布
        gImage.fillRect(0, 0, width, height);
        //改变画笔的颜色
        gImage.setColor(Color.orange);
        //改变文字大小和样式
        gImage.setFont(new Font("正楷",Font.BOLD,50));

        if(state == 0){
            //添加文字
            gImage.drawString("选择游戏模式",220,100);
            gImage.drawString("单人游戏",220,200);
            gImage.drawString("双人游戏",220,300);
            gImage.drawString("按1，2选择模式，按回车开始游戏",0,400);
            gImage.drawImage(select,160,y,null);
        }
        else if(state == 1||state == 2){
            gImage.setColor(Color.red);
            gImage.setFont(new Font("仿宋",Font.BOLD,20));
            gImage.drawString("WASD控制移动",0,510);
            gImage.drawString("空格射击",0,550);
            if(state == 2){
                gImage.drawString("方向键控制移动",575,510);
                gImage.drawString("K射击",575,550);
            }

            //paint重绘游戏元素
            for(Tank tank : tankList){
                tank.paintSelf(gImage);
            }
            for(Bullet bullet: bulletList){
                bullet.paintSelf(gImage);
            }
            bulletList.removeAll(removeList);
            for(Bot bot: botList){
                bot.paintSelf(gImage);
            }
            for (Wall wall: wallList){
                wall.paintSelf(gImage);
            }
            for(Base base : baseList){
                base.paintSelf(gImage);
            }
            for(BlastObj blast : blastList){
                blast.paintSelf(gImage);
            }
            //重绘次数+1
            count++;
        }
        else if(state == 3){
            gImage.drawString("游戏暂停",220,200);
        }
        else if(state == 4){
            gImage.drawString("游戏失败",220,200);
        }
        else if(state == 5){
            gImage.drawString("游戏胜利",220,200);
        }
        /* 将缓冲区绘制好的图形整个绘制到容器的画布中 */
        g.drawImage(offScreenImage, 0, 0, null);
    }

    private class KeyMonitor extends KeyAdapter {

        @Override
        public void keyPressed(KeyEvent e) {
            //super.keyPressed(e);
            int key = e.getKeyCode();
            switch (key){
                case KeyEvent.VK_1:
                    y = 150;
                    a = 1;
                    break;
                case KeyEvent.VK_2:
                    y = 250;
                    a = 2;
                    break;
                case KeyEvent.VK_ENTER:
                    state = a;
                    //添加玩家
                    if(state == 1 && !start){
                        tankList.add(playerOne);
                    }else{
                        tankList.add(playerOne);
                        tankList.add(playerTwo);
                    }
                    start = true;
                    break;
                case KeyEvent.VK_P:
                    if(state != 3){
                        a = state;
                        state = 3;
                    }
                    else{
                        state = a;
                        if(a == 0) {
                            a = 1;
                        }
                    }
                    break;
                default:
                    playerOne.keyPressed(e);
                    playerTwo.keyPressed(e);
                    break;
            }
        }

        @Override
        public void keyReleased(KeyEvent e){
            playerOne.keyReleased(e);
            playerTwo.keyReleased(e);
        }
    }

    public static void main(String[] args) {
        GamePanel gamePanel = new GamePanel();
        gamePanel.launch();
    }
}
```

##  GameObject

GameObject() 

GameObject(Image, int, int, GamePanel) 

getImg(): Image 

setImg(Image): void 

对X,Y,Width,Height,Speed,GamePanel 的getter和setter

paintSelf(Graphics): void 

getRec(): Rectangle

```java
package com.company.tank;

import java.awt.*;

public abstract class GameObject {

    //游戏元素图片
    Image img;
    //游戏元素的横坐标
    int x;
    //游戏元素的纵坐标
    int y;
    //游戏元素的宽
    int width;
    //游戏元素的高
    int height;
    //游戏元素的移动速度
    int speed;
    //游戏元素的移动方向
    Direction direction;
    //引入主界面
    GamePanel gamePanel;

    public GameObject(){}
    public GameObject(Image img, int x, int y, GamePanel gamePanel) {
        this.img = img;
        this.x = x;
        this.y = y;
        this.gamePanel = gamePanel;
    }

    public Image getImg() {
        return img;
    }

    public void setImg(Image img) {
        this.img = img;
    }

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }

    public int getY() {
        return y;
    }

    public void setY(int y) {
        this.y = y;
    }

    public int getWidth() {
        return width;
    }

    public void setWidth(int width) {
        this.width = width;
    }

    public int getHeight() {
        return height;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public double getSpeed() {
        return speed;
    }

    public void setSpeed(int speed) {
        this.speed = speed;
    }

    public GamePanel getGamePanel() {
        return gamePanel;
    }

    public void setGamePanel(GamePanel gamepanel) {
        this.gamePanel = gamePanel;
    }

    //继承元素绘制自己的方法
    public abstract void paintSelf(Graphics g);

    //获取当前游戏元素的矩形,是为碰撞检测而写（位置）
    public abstract Rectangle getRec();
}

```

## Base

Base(Image, int, int, GamePanel) 

paintSelf(Graphics): void 

getRec(): Rectangle

```java
package com.company.tank;

import java.awt.*;

public class Base extends GameObject {
    public int width = 60;
    public int height = 60;
    public Base(Image img, int x, int y, GamePanel gamePanel){
        super(img, x, y, gamePanel);
    }
    @Override
    public void paintSelf(Graphics g) {
        g.drawImage(img, x, y, null);
    }
    @Override
    public Rectangle getRec() {
        return new Rectangle(x, y, width, height);
    }
}


```

## Wall

Wall(Image, int, int, GamePanel) 

paintSelf(Graphics): void 

getRec(): Rectangle

```java
package com.company.tank;
import java.awt.*;

public class Wall extends GameObject {

    public int width = 60;
    public int height = 60;
    public Wall(Image img, int x, int y, GamePanel gamePanel){
        super(img, x, y, gamePanel);
    }

    @Override
    public void paintSelf(Graphics g) {
        g.drawImage(img, x, y, null);
    }

    @Override
    public Rectangle getRec() {
        return new Rectangle(x, y, width, height);
    }
}


```

## Tank

AttackCD 

run(): void 

Tank(Image, int, int, Image, Image, Image, Image, ...) 

leftward(): void  //移动

rightward(): void 

upward(): void 

downward(): void 

attack(): void 

hitWall(int, int): boolean 

moveToBorder(int, int): boolean 

getHeadPoint(): Point 

paintSelf(Graphics): void 

getRec(): Rectangle

```java
package com.company.tank;



import java.awt.*;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.util.List;

public class Tank extends GameObject{

    private boolean attackCoolDown =true;//攻击冷却状态
    private int attackCoolDownTime =1000;//攻击冷却时间毫秒间隔1000ms发射子弹
    private Image upImage; //向上移动时的图片
    private Image downImage;//向下移动时的图片
    private Image rightImage;//向右移动时的图片
    private Image leftImage;//向左移动时的图片
    boolean alive = true;
    //坦克size
    int width = 40;
    int height = 50;
    //坦克初始方向
    Direction direction = Direction.UP;
    //坦克速度
    private int speed = 3;
    //坦克头部坐标
    Point p;

    //坦克坐标，方向，图片，方向，面板
    public Tank(Image img, int x, int y, Image upImage, Image downImage, Image leftImage, Image rightImage, GamePanel gamePanel) {
        super(img, x, y, gamePanel);
        this.upImage = upImage;
        this.leftImage = leftImage;
        this.downImage = downImage;
        this.rightImage = rightImage;
    }

    public void leftward(){
        direction = Direction.LEFT;
        setImg(leftImage);
        if(!hitWall(x-speed, y) && !moveToBorder(x-speed, y) && alive){
            this.x -= speed;
        }
    }
    public void rightward(){
        direction = Direction.RIGHT;
        setImg(rightImage);
        if(!hitWall(x+speed, y) && !moveToBorder(x+speed, y) && alive){
            this.x += speed;
        }
    }
    public void upward(){
        direction = Direction.UP;
        setImg(upImage);
        if(!hitWall(x, y-speed) && !moveToBorder(x, y- speed) && alive){
            this.y -= speed;
        }
    }
    public void downward(){
        direction = Direction.DOWN;
        setImg(downImage);
        if(!hitWall(x, y+speed) && !moveToBorder(x, y+speed) && alive){
            this.y += speed;
        }
    }

    public void attack(){
        Point p = getHeadPoint();
        if(attackCoolDown && alive){
            Bullet bullet = new Bullet(Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/bullet/bulletGreen.gif")),p.x,p.y,direction, this.gamePanel);
            this.gamePanel.bulletList.add(bullet);
            attackCoolDown = false;
            new AttackCD().start();
        }
    }

    public boolean hitWall(int x, int y){
        //假设玩家坦克前进，下一个位置形成的矩形
        Rectangle next = new Rectangle(x, y, width, height);
        //地图里所有的墙体
        List<Wall> walls = this.gamePanel.wallList;
        //判断两个矩形是否相交（即是否撞墙）
        for(Wall w:walls){
            if(w.getRec().intersects(next)){
                return true;
            }
        }
        return false;
    }

    public boolean moveToBorder(int x, int y){
        if(x < 0){
            return true;
        }else if(x > this.gamePanel.getWidth()-width){
            return true;
        }
        if(y < 0){
            return true;
        }else if(y > this.gamePanel.getHeight()-height){
            return true;
        }
        return false;
    }

    public class AttackCD extends Thread{
        public void run(){
            attackCoolDown=false;//将攻击功能设置为冷却状态
            try{
                Thread.sleep(attackCoolDownTime);//休眠1秒
            }catch(InterruptedException e){
                e.printStackTrace();
            }
            attackCoolDown=true;//将攻击功能解除冷却状态
            this.interrupt();
        }
    }

    //根据方向确定头部位置，x和y是左下角的点
    public Point getHeadPoint(){
        switch (direction){
            case UP:
                return new Point(x + width/2, y );
            case LEFT:
                return new Point(x, y + height/2);
            case DOWN:
                return new Point(x + width/2, y + height);
            case RIGHT:
                return new Point(x + width, y + height/2);
            default:
                return null;
        }
    }

    @Override
    public void paintSelf(Graphics g) {
        g.drawImage(img, x, y, null);
    }

    @Override
    public Rectangle getRec() {
        return new Rectangle(x, y, width, height);
    }
}


```

## PlayerOne

PlayerOne(Image, int, int, Image, Image, Image, Image, ...) 

keyPressed(KeyEvent): void 

keyReleased(KeyEvent): void 

move(): void 

paintSelf(Graphics): void 

getRec(): Rectangle

```java
package com.company.tank;

import java.awt.*;
import java.awt.event.KeyEvent;

public class PlayerOne extends Tank {
    private boolean up = false;
    private boolean left = false;
    private boolean right = false;
    private boolean down = false;

    public PlayerOne(Image img, int x, int y, Image upImage, Image downImage, Image leftImage, Image rightImage, GamePanel gamePanel){
        super(img, x, y, upImage, downImage, leftImage, rightImage, gamePanel);
    }

    public void keyPressed(KeyEvent e){
        int key = e.getKeyCode();
        switch (key){
            case KeyEvent.VK_A:
                left = true;
                break;
            case KeyEvent.VK_S:
                down = true;
                break;
            case KeyEvent.VK_D:
                right = true;
                break;
            case KeyEvent.VK_W:
                up = true;
                break;
            case KeyEvent.VK_SPACE:
                this.attack();
                break;
            case KeyEvent.VK_G: // 开挂：清除现存敌方坦克中存活最久的一个
                Bot b = this.gamePanel.botList.remove(0);
                gamePanel.botList.remove(b);
                gamePanel.blastList.add(new BlastObj(b.x, b.y));
                break;
                //TODO g
            default:
                break;
        }
    }

    public void keyReleased(KeyEvent e){
        int key = e.getKeyCode();
        switch (key){
            case KeyEvent.VK_A:
                left = false;
                break;
            case KeyEvent.VK_S:
                down = false;
                break;
            case KeyEvent.VK_D:
                right = false;
                break;
            case KeyEvent.VK_W:
                up = false;
                break;
            default:
                break;
        }
    }

    public void move(){ // 根据状态前进/停止
        if(left){
            leftward();
        }
        else if(right){
            rightward();
        }
        else if(up){
            upward();
        }else if(down){
            downward();
        }
    }

    public void paintSelf(Graphics g) {
        g.drawImage(img, x, y, null);
        move();
    }

    public Rectangle getRec() {
        return new Rectangle(x, y, width, height);
    }
}

```

==添加player只需要改名==

## Bot

Bot(Image, int, int, Image, Image, Image, Image, ...) 

go(): void 

randomDirection(): Direction 

attack(): void 

paintSelf(Graphics): void 

getRec(): Rectangle

```java
package com.company.tank;
import java.awt.*;
import java.util.Random;

public class Bot extends Tank{
    int moveTime = 0;
    public Bot(Image img, int x, int y, Image upImage, Image downImage, Image leftImage, Image rightImage, GamePanel gamePanel) {
        super(img, x, y, upImage, downImage, leftImage, rightImage, gamePanel);
    }

    public void go(){
        attack();
        if(moveTime>=20) {
            direction=randomDirection();
            moveTime=0;
        }else {
            moveTime+=1;
        }
        switch (direction) {
            case UP -> upward();
            case DOWN -> downward();
            case RIGHT -> rightward();
            case LEFT -> leftward();
        }
    }

    //电脑坦克随机方向
    public Direction randomDirection() {
        Random r = new Random();
        int rnum = r.nextInt(4);
        switch(rnum) {
            case 0:
                return Direction.UP;
            case 1:
                return Direction.RIGHT;
            case 2:
                return Direction.LEFT;
            default:
                return Direction.DOWN;
        }
    }

    //只有4%几率攻击
    public void attack() {
        Point p = getHeadPoint();
        Random r = new Random();
        int rnum =r.nextInt(100);
        if(rnum<4) {
            EnemyBullet enemyBullet = new EnemyBullet(Toolkit.getDefaultToolkit().getImage(getClass().getResource("images/bullet/bulletYellow.gif")),p.x,p.y,direction,gamePanel);
            this.gamePanel.bulletList.add(enemyBullet);
        }
    }

    @Override
    public void paintSelf(Graphics g) {
        g.drawImage(img,x,y,null);
        this.go();
    }

    @Override
    public Rectangle getRec() {
        return new Rectangle(x, y, width, height);
    }
}


```

## Bullet

Bullet(Image, int, int, Direction, GamePanel) 

go(): void 

leftward(): void 

rightward(): void 

upward(): void 

downward(): void 

hitBot(): void   //碰撞检测

hitBase(): void 

hitWall(): void 

moveToBorder(): void 

paintSelf(Graphics): void 

getRec(): Rectangle

```java
package com.company.tank;


import java.awt.*;
import java.util.List;

public class Bullet extends GameObject{
    private  int width = 10;
    private int height = 10;
    private int speed = 7;
    Direction direction;

    public Bullet(Image img, int x, int y, Direction direction,GamePanel gamePanel) {
        super(img, x,  y, gamePanel);
        this.direction = direction;
    }

    public void go(){
        /*判断移动方向*/
        switch (direction) {
            case UP -> upward();
            case LEFT -> leftward();
            case DOWN -> downward();
            case RIGHT -> rightward();
        }
    }

    // 子弹移动+边界判断
    public void leftward(){
        x -= speed;
        moveToBorder();
    }
    public void rightward(){
        x += speed;
        moveToBorder();
    }
    public void upward(){
        y -= speed;
        moveToBorder();
    }
    public void downward(){
        y += speed;
        moveToBorder();
    }

    /*子弹与坦克碰撞检测*/
    public void hitBot(){
        Rectangle next= this.getRec();
        List<Bot> bots = this.gamePanel.botList;

        //子弹和bot
        for(Bot bot: bots){
            if(bot.getRec().intersects(next)){ // 空间有交集
                this.gamePanel.blastList.add(new BlastObj(bot.x-34, bot.y-14));
                this.gamePanel.botList.remove(bot);
                this.gamePanel.removeList.add(this); // 回收子弹
                break;
            }
        }
    }

    public void hitBase(){
        Rectangle next = this.getRec();
        for(Base base: gamePanel.baseList) {
            if (base.getRec().intersects(next)) {
                this.gamePanel.baseList.remove(base);
                this.gamePanel.removeList.add(this);
                this.gamePanel.state = 4;
                break;
            }
        }
    }

    public void hitWall(){
        Rectangle next = this.getRec();
        List<Wall> walls = this.gamePanel.wallList;
        for(Wall w: walls) {
            if (w.getRec().intersects(next)) {
                this.gamePanel.wallList.remove(w);
                this.gamePanel.removeList.add(this);
                break;
            }
        }
    }

    // 出界回收
    public void moveToBorder(){
        if (x < 0||x > this.gamePanel.getWidth()) {
            this.gamePanel.removeList.add(this);
        }
        if(y < 0||y > this.gamePanel.getHeight()) {
            this.gamePanel.removeList.add(this);
        }
    }

    @Override
    public void paintSelf(Graphics g) {
        g.drawImage(img, x, y, null);
        go();
        //碰撞检测
        hitBot();
        hitWall();
        hitBase();
    }

    @Override
    public Rectangle getRec() {
        return new Rectangle(x, y, width, height);
    }
}


```

## EnemyBullet

EnemyBullet(Image, int, int, Direction, GamePanel) 

hitTank(): void 

paintSelf(Graphics): void

```java
package com.company.tank;


import java.awt.*;
import java.util.List;

public class EnemyBullet extends Bullet {
    public EnemyBullet(Image img, int x, int y, Direction direction,GamePanel gamePanel){
        super(img, x, y, direction, gamePanel);
    }

    public void hitTank(){
        Rectangle next= this.getRec();
        java.util.List<Tank> tanks = this.gamePanel.tankList;
        //子弹和Tank
        for(Tank tank: tanks){
            if(tank.getRec().intersects(next)){
                tank.alive = false;
                this.gamePanel.blastList.add(new BlastObj(tank.x-34, tank.y-14));
                this.gamePanel.tankList.remove(tank);
                this.gamePanel.removeList.add(this);
                break;

            }
        }
    }

    public void paintSelf(Graphics g){
        g.drawImage(img, x, y, null);
        go();
        hitBase();
        hitWall();
        hitTank();
    }
}


```

## BlastObj

BlastObj() 

BlastObj(int, int) 

init(): void 

paintSelf(Graphics): void 

getRec(): Rectangle

```java
package com.company.tank;


import java.awt.*;

public class BlastObj extends GameObject {

    static Image[] imgs = new Image[3];

    int explodeCount = 0;

    public static void init() { // 初始化
        for (int i = 0; i < 3; i++) {
            imgs[i] = Toolkit.getDefaultToolkit().getImage(BlastObj.class.getResource("images/blast/blast" +(i + 1)+".png"));
        }
    }

    public BlastObj() {
        super();
    }

    public BlastObj(int x, int y) {
        this.x = x;
        this.y = y;
        init();
    }

    @Override
    public void paintSelf(Graphics g) {
        //绘制点击爆炸效果(连续绘制)
        if (explodeCount < 3 && explodeCount>=0){
            g.drawImage(imgs[explodeCount],x,y,null);
            explodeCount++;
        }
    }

    @Override
    public Rectangle getRec() {
        return null;
    }
}
```

## Direction

```java
public enum Direction {
    UP,LEFT,RIGHT,DOWN
}
```

## 图片添加

![image-20220405182743934](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220405182743934.png)

==放在对应的out目录下即可用相对路径引用==

# 最后

不定时更新，以后会重构。

