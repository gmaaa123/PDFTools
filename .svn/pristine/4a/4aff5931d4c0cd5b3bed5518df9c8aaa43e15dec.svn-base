using iTextSharp.text;
using iTextSharp.text.pdf;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace PDFTools
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            this.Text = "PDF工具箱 1.0.0 Beta";
        }

        //默认打开路径
        private string InitialDirectory = "c:\\";
        private string filePath = null;
        private int pageNum = 0;

        //统一对话框
        private void InitialDialog()
        {
            OpenFileDialog fileDialog = new OpenFileDialog();
            fileDialog.InitialDirectory = InitialDirectory;//初始化路径
            fileDialog.Filter = "PDF文件(*.pdf)|*.pdf";//过滤选项设置，PDF文件，所有文件。
            fileDialog.FilterIndex = 2;//当前使用第二个过滤字符串
            fileDialog.RestoreDirectory = true;//对话框关闭时恢复原目录
            fileDialog.Title = "请选择要处理的文件";
            if (fileDialog.ShowDialog() == DialogResult.OK)
            {
                for (int i = 1; i <= fileDialog.FileName.Length; i++)
                {
                    if (fileDialog.FileName.Substring(fileDialog.FileName.Length - i, 1).Equals(@"\"))
                    {
                        //更改默认路径为最近打开路径
                        InitialDirectory = fileDialog.FileName.Substring(0, fileDialog.FileName.Length - i + 1);
                      
                      
                    }
                }
                filePath = fileDialog.FileName;
                Document document = new Document();
                PdfReader reader = new PdfReader(filePath);
                pageNum = reader.NumberOfPages;
                reader.Close(); //不关闭会一直占用pdf资源，对接下来的操作会有影
                textBox3.Text = filePath;
                PageNumlabel.Text = "总共"+ pageNum + "页";
               

            }
            else
            {
               
            }
        }

        private void label1_Click(object sender, EventArgs e)
        {

        }

        private void textBox2_TextChanged(object sender, EventArgs e)
        {

        }

        private void button2_Click(object sender, EventArgs e)
        {
            Resultlabel.Text = "";

            //初始化文件选择框
            InitialDialog();
           

        }

        private void label3_Click(object sender, EventArgs e)
        {

        }

        private void button1_Click(object sender, EventArgs e)
        {
            Resultlabel.Text = "";

            if (string.IsNullOrEmpty(filePath))
            {
                Resultlabel.Text = "请选择需要处理的PDF文件!";
                Resultlabel.ForeColor = System.Drawing.Color.Red;
                return;
            }

            if (!File.Exists(filePath))
            {

                Resultlabel.Text = "选择的文件不存在!";
                Resultlabel.ForeColor = System.Drawing.Color.Red;
                return;
            }

            if (!radioButton1.Checked & !radioButton2.Checked & !radioButton3.Checked & !radioButton4.Checked)
            {
                Resultlabel.Text = "请选择分割方式!";
                Resultlabel.ForeColor = System.Drawing.Color.Red;
                return;
            }

            PdfExtractorUtility pdf = new PdfExtractorUtility();

            if (radioButton1.Checked)
            {
                pdf.Split2SinglePage(filePath);
                Resultlabel.Text = "操作成功!";
                Resultlabel.ForeColor = System.Drawing.Color.Green;
            }


            if (radioButton2.Checked)
            {
                int page = int.Parse(textBox1.Text);
                if (page > pageNum)
                {
                    Resultlabel.Text = "单个文档页数不能大于文件总页数!";
                    Resultlabel.ForeColor = System.Drawing.Color.Red;
                    return;
                }
                pdf.Split2Page(filePath, page);
                Resultlabel.Text = "操作成功!";
                Resultlabel.ForeColor = System.Drawing.Color.Green;
            }

            if (radioButton3.Checked)
            {
                int count = int.Parse(textBox2.Text);
                LogUtil.WriteLog("平均分割数量 " + count.ToString());
                if (count > pageNum)
                {
                    Resultlabel.Text = "分割数量不能大于文件总页数!";
                    Resultlabel.ForeColor = System.Drawing.Color.Red;
                    return;
                }
                pdf.Split2AveragePage(filePath, count);
                Resultlabel.Text = "操作成功!";
                Resultlabel.ForeColor = System.Drawing.Color.Green;
            }

            if (radioButton4.Checked)
            {
                Resultlabel.Text = "";

                if (string.IsNullOrEmpty(StringUtil.GetString(textBox4.Text, ",", false))|| string.IsNullOrEmpty(StringUtil.GetString(textBox4.Text, "&", false)))
                {
                    Resultlabel.Text = "自定义范围内容不能为空 或者 自定义内容有问题！";
                    Resultlabel.ForeColor = System.Drawing.Color.Red;
                    return;
                }

                if (textBox4.Text.Contains("，"))
                {
                    Resultlabel.Text = "请使用英文输入法的逗号！";
                    Resultlabel.ForeColor = System.Drawing.Color.Red;
                    return;
                }
                pdf.SplitPDFCustPage(filePath, textBox4.Text);
                Resultlabel.Text = "操作成功!";
                Resultlabel.ForeColor = System.Drawing.Color.Green;
            }
        }
        private void radioButton1_CheckedChanged(object sender, EventArgs e)
        {
            Resultlabel.Text = "";
            textBox1.Text = "";
            textBox2.Text = "";

        }

        private void radioButton2_CheckedChanged(object sender, EventArgs e)
        {
            Resultlabel.Text = "";
            textBox1.Text = "";
            textBox2.Text = "";
        }

        private void radioButton3_CheckedChanged(object sender, EventArgs e)
        {
            Resultlabel.Text = "";
            textBox1.Text = "";
            textBox2.Text = "";
        }

        private void PageNumlabel_Click(object sender, EventArgs e)
        {

        }

        
        private void textBox2_KeyPress(object sender, KeyPressEventArgs e)
        {
            //只能输入正整数，首位不能为0
            if (e.KeyChar == 0x20) e.KeyChar = (char)0;  //禁止空格键
            if (e.KeyChar != '\b')//这是允许输入退格键 
            {
                int len = textBox2.Text.Length;
                if (len < 1 && e.KeyChar == '0') //不允许输入0
                {
                    e.Handled = true;
                }
                else if ((e.KeyChar < '0') || (e.KeyChar > '9'))//这是允许输入0-9数字 
                {
                    e.Handled = true;
                }

            }
        }

        private void textBox1_KeyPress(object sender, KeyPressEventArgs e)
        {
            if (e.KeyChar == 0x20) e.KeyChar = (char)0;  //禁止空格键
            if (e.KeyChar != '\b')//这是允许输入退格键 
            {
                int len = textBox1.Text.Length; 
                if (len < 1 && e.KeyChar == '0') //不允许输入0
                {
                    e.Handled = true;
                }
                else if ((e.KeyChar < '0') || (e.KeyChar > '9'))//这是允许输入0-9数字 
                {
                    e.Handled = true;
                }

            }
        }

        private void textBox1_TextChanged(object sender, EventArgs e)
        {

        }
    }
}
