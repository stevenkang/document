# 漂亮的验证码
 
效果图：![](https://i.loli.net/2018/05/11/5af559743bf1e.jpg)
 
JSP 源代码：
```jsp
<%@ page contentType="image/JPEG"
    import="java.awt.*,java.awt.image.*,java.util.*,javax.imageio.*"
    pageEncoding="utf-8"%><%! Color getRandColor(int fc, int bc) {//给定范围获得随机颜色
        Random random = new Random();
        if (fc > 255)
            fc = 255;
        if (bc > 255)
            bc = 255;
        int r = fc + random.nextInt(bc - fc);
        int g = fc + random.nextInt(bc - fc);
        int b = fc + random.nextInt(bc - fc);
        return new Color(r, g, b);
    }%><%
    //设置页面不缓存
    response.setHeader("Pragma", "No-cache");
    response.setHeader("Cache-Control", "no-cache");
    response.setDateHeader("Expires", 0);
 
    //候选字符
    String codes = "1234567890abcdefghijklmnopqrstuvwxyz"
          +"ABCDEFGHIJKLMNOPQRSTUVWXYZ";
   
    // 在内存中创建图象
    int width = 130, height = 53;
    BufferedImage image = new BufferedImage(width, height,
            BufferedImage.TYPE_INT_RGB);
 
    // 获取图形上下文
    Graphics g = image.getGraphics();
 
    //生成随机类
    Random random = new Random();
 
    // 设定背景色
    g.setColor(getRandColor(237, 255));
    g.fillRect(0, 0, width, height);
 
    //设定字体
    g.setFont(new Font("Verdana", Font.PLAIN, 36));
 
    //画边框
    g.setColor(new Color(220,250,240));
    g.drawRect(0,0,width-1,height-1);
   
    // 随机产生155条干扰线，使图象中的认证码不易被其它程序探测到
    g.setColor(getRandColor(160, 220));
    for (int i = 0; i < 10; i++) {
        int x = random.nextInt(width);
        int y = random.nextInt(height);
        int xl = random.nextInt(64);
        int yl = random.nextInt(64);
        g.drawLine(x, y, x + xl, y + yl);
    }
   
    // 取随机产生的认证码(4位数字)
    String sRand = "";
    for (int i = 0; i < 4; i++) {
        Integer rIndex = random.nextInt(codes.length());
        String rand = String.valueOf(codes.charAt(rIndex));
        sRand += rand;
        // 将认证码显示到图象中
        int wcolor = random.nextInt(3);
        int wr = 1, wg = 1, wb = 1, ws = 120;//调整ws可以让字体变得更好看
        switch (wcolor) {
         case 0:
          wr = 200 + random.nextInt(55);
          wg = ws + random.nextInt(55);
          wb = ws + random.nextInt(55);
          break;
         case 1:
          wr = ws + random.nextInt(55);
          wg = 200 + random.nextInt(55);
          wb = ws + random.nextInt(55);
          break;
         case 2:
          wr = ws + random.nextInt(55);
          wg = ws + random.nextInt(55);
          wb = 200 + random.nextInt(55);
          break;
        }
        g.setColor(new Color(wr, wg, wb));//调用函数出来的颜色相同，可能是因为种子太接近，所以只能直接生成
        g.drawString(rand, 25 * i + random.nextInt(15) + 10, 30+random.nextInt(20));
    }
 
    // 将认证码存入SESSION
    session.setAttribute("code", sRand);
 
    // 图象生效
    g.dispose();
 
    // 输出图象到页面
    ImageIO.write(image, "JPEG", response.getOutputStream());
%>
```
