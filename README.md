	# c-sharp-snake
	//第一次使用github
	using System;
	using System.Collections.Generic;
	using System.ComponentModel;
	using System.Data;
	using System.Drawing;
	using System.Linq;
	using System.Text;
	using System.Threading.Tasks;
	using System.Windows.Forms;

	namespace 贪吃蛇
	{
		public partial class Form1 : Form
		{
			public int score = 5;//设置分数
			string keyname = "start";
			Label[] snakearray = new Label[100];//设置一个用于存放蛇身的数组
			Random snakerandom = new Random();
			int a = 0;
			int b = 0;
			public Form1()
			{
				InitializeComponent();
			}
			private void Form1_Load(object sender, EventArgs e)
			{
				//设置背景大小颜色
				this.Top = 120;
				this.Left = 120;
				this.Width = 800;
				this.Height = 600;
				this.BackColor = Color.Black;
				//造蛇身体
				for (int i = 0; i <= 5; i++)
				{
					Label snakebody = new Label();
					snakebody.Width = snakebody.Height = 20;
					snakebody.Top = 400;
					snakebody.Left = 400 - i * 20;
					snakebody.BackColor = Color.Blue;
					snakebody.Text = "*";
					snakebody.Font = new System.Drawing.Font("宋体", 18);
					snakebody.Tag = i;
					snakearray[i] = snakebody;
					this.Controls.Add(snakebody);
				}
				timer1.Tick += new EventHandler(timer1Tick);
				this.KeyDown += new KeyEventHandler(Form1KeyDown);
				snakefood();
				timer1.Start();
			}
			void timer1Tick(object sender, EventArgs e)//时间函数
			{
				int hhh = score - 5;
				this.Text = "分数：" + hhh;
				int x1, y1;
				x1 = snakearray[0].Left;
				y1 = snakearray[0].Top;
				if (keyname == "start")
				{
					snakearray[0].Left = x1 + 20;
					snakemove(x1, y1);
				}
				if (keyname == "Right")
				{

					snakearray[0].Left = x1 + 20;
					snakemove(x1, y1);
				}
				if (keyname == "Left")
				{
					snakearray[0].Left = x1 - 20;
					snakemove(x1, y1);
				}
				if (keyname == "Up")
				{
					snakearray[0].Top = y1 - 20;
					snakemove(x1, y1);
				}
				if (keyname == "Down")
				{
					snakearray[0].Top = y1 + 20;
					snakemove(x1, y1);
				}
				//穿墙
				if (y1 > 600)
				{
					snakearray[0].Top = 0;
				}
				if (y1 < 0)
				{
					snakearray[0].Top = 600;
				}
				if (x1 > 800)
				{
					snakearray[0].Left = 0;
				}
				if (x1 < 0)
				{
					snakearray[0].Left = 800;
				}
				eattime();
			}
			void Form1KeyDown(object sender, KeyEventArgs e)//监听键盘
			{
				int x, y;
				x = snakearray[0].Left;
				y = snakearray[0].Top;
				keyname = e.KeyCode.ToString();
				if (e.KeyCode.ToString() == "Right")
				{
					snakearray[0].Left = x + 20;
					snakemove(x, y);
				}
				if (e.KeyCode.ToString() == "Left")
				{
					snakearray[0].Left = x - 20;
					snakemove(x, y);
				}
				if (e.KeyCode.ToString() == "Up")
				{
					snakearray[0].Top = y - 20;
					snakemove(x, y);
				}
				if (e.KeyCode.ToString() == "Down")
				{
					snakearray[0].Top = y + 20;
					snakemove(x, y);
				}
				eattime();
			}
			void snakemove(int x1, int y1)//蛇的移动
			{
				int xx = 0;
				int yy = 0;
				for (int i = 1; snakearray[i] != null; i++)
				{
					if (i >= 3)
					{
						xx = a;
						yy = b;
					}
					if (i == 1)
					{
						xx = snakearray[i].Left;
						yy = snakearray[i].Top;
						snakearray[i].Left = x1;
						snakearray[i].Top = y1;
					}
					else
					{
						a = snakearray[i].Left;
						b = snakearray[i].Top;
						snakearray[i].Left = xx;
						snakearray[i].Top = yy;
                }
            }
        }
        void snakefood()//生成蛇的食物
        {
            double xx = snakearray[0].Left;
            double yy = snakearray[0].Top;
            Label snakefood = new Label();
            snakefood.Width = 20;
            snakefood.Height = 20;
            snakefood.Top = snakerandom.Next(1, 28) * 20;
            snakefood.Left = snakerandom.Next(1, 28) * 20;
            snakefood.Tag = "food";
            snakefood.BackColor = Color.Yellow;
            this.Controls.Add(snakefood);
        }
        void snakeeat()//吃掉食物以后
        {
            Label snakebody = new Label();
            snakebody.Width = snakebody.Height = 20;
            snakebody.Top = b;
            snakebody.Left = a;
            snakebody.BackColor = Color.Blue;
            snakebody.Text = "@";
            snakebody.Font = new System.Drawing.Font("宋体", 18);
            snakebody.Tag = score + 1;
            snakearray[score + 1] = snakebody;
            this.Controls.Add(snakebody);
            score = score + 1;

        }
        void eattime()//判断是否蛇头和食物碰撞
        {
            double x1 = 20, y1 = 20, x2 = 20, y2 = 20;
            foreach (Label snakelabel in this.Controls)
            {
                if (snakelabel.Tag.ToString() == "food".ToString())
                {
                    x2 = snakelabel.Left;
                    y2 = snakelabel.Top;
                }
                if (snakelabel.Tag.ToString() == "0".ToString())
                {
                    x1 = snakelabel.Left;
                    y1 = snakelabel.Top;
                }
            }
            if (x2 == x1 && y2 == y1)//如果蛇头和食物重合触发snakeeat方法并重新随机生成食物
            {
                snakeeat();
                foreach (Label snakelabel in this.Controls)
                {
                    if (snakelabel.Tag.ToString() == "food".ToString())
                    {
                        snakelabel.Top = snakerandom.Next(1, 28) * 20;
                        snakelabel.Left = snakerandom.Next(1, 28) * 20;
                    }
                }
            }
        }
    }
