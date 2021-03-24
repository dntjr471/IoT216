#210323
## 데이터베이스 수업 1
```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Data.SqlClient;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace DBManager
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void mnuMigration_Click(object sender, EventArgs e)
        {
            DialogResult ret = openFileDialog1.ShowDialog();    // new DialogResult()
            if (ret != DialogResult.OK) return;
            string nFile = openFileDialog1.FileName;    //  full name

            StreamReader sr = new StreamReader(nFile);

            //--------------------------------------------
            //      Header 처리 프로세스
            //--------------------------------------------

            string buf = sr.ReadLine();     // 1 Line read  :   Header Line
            if (buf == null) return;
            string[] sArr = buf.Split(',');
            for (int i = 0; i < sArr.Length; i++)
            {
                dataGrid.Columns.Add(sArr[i], sArr[i]);
            }

            //--------------------------------------------
            //      Row 데이터 처리 프로세스
            //--------------------------------------------

            while (true)     // c#에선 true 
            {
                buf = sr.ReadLine();
                if (buf == null) break;
                sArr = buf.Split(',');
                //dataGrid.Rows.Add(sArr);
                int rowIdx = dataGrid.Rows.Add();    // 1 Line 생성
                for (int i = 0; i < sArr.Length; i++)
                {
                    dataGrid.Rows[rowIdx].Cells[i].Value = sArr[i];
                }
            }
        }

        SqlConnection sqlCon = new SqlConnection();
        SqlCommand sqlCmd = new SqlCommand();
        string sConn = @"Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=;Integrated Security=True;Connect Timeout=30";
        private void mnuDBOpen_Click(object sender, EventArgs e)
        {
            try// 예외처리
            {
                DialogResult ret = openFileDialog1.ShowDialog();    // db file
                if (ret != DialogResult.OK) return;
                string nFile = openFileDialog1.FileName;    //  full name
                string[] ss = sConn.Split(';');

                sqlCmd.Connection = sqlCon;
                sqlCon.ConnectionString = $"{ss[0]};{ss[1]}{nFile};{ss[2]};{ss[3]}";
                sqlCon.Open();
                sbPanel1.Text = "DB Opened success." ;
                sbPanel1.BackColor = Color.LightGreen;
                sbPanel2.Text = openFileDialog1.SafeFileName;   // 파일 이름만 불러옴
                
            }
            catch(SqlException e1)
            {
                MessageBox.Show(e1.Message);
                sbPanel1.Text = "DB cannot open.";
                sbPanel1.BackColor = Color.Red;
            }
        }

        int RunSql(string sql)
        {
            try
            {
                sqlCmd.CommandText = sql;
                sqlCmd.ExecuteNonQuery();   //  select 문 제외 -> no return value
                                            // update, insert, delete, create, alt
                //sqlCmd.ExecuteReader();
            }
            catch (SqlException e1)
            {
                MessageBox.Show(e1.Message);
            }
            catch(InvalidOperationException e2)
            {
                MessageBox.Show(e2.Message);
            }
            return 0;
        }
        private void mnuExecSql_Click(object sender, EventArgs e)
        {
            RunSql(tbSql.Text);

        }

        private void mnuSelSql_Click(object sender, EventArgs e)
        {
            RunSql(tbSql.SelectedText);
            //string sql = tbSql.SelectedText;
            //sqlCmd.CommandText = sql;
            //sqlCmd.ExecuteNonQuery();
        }
    }
}
