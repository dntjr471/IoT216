```
# 210324
## 데이터베이스 수업 2일차

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
                int rIdx = dataGrid.Rows.Add();    // 1 Line 생성
                for (int i = 0; i < sArr.Length; i++)
                {
                    dataGrid.Rows[rIdx].Cells[i].Value = sArr[i];
                }
            }

            //--------------------------------------------
            //      Column 데이터 처리 프로세스
            //--------------------------------------------

            while (true)     // c#에선 true 
            {
                buf = sr.ReadLine();
                if (buf == null) break;
                sArr = buf.Split(',');
                //dataGrid.Rows.Add(sArr);
                int rIdx = dataGrid.Rows.Add();    // 1 Line 생성
                for (int i = 0; i < sArr.Length; i++)
                {
                    dataGrid.Rows[rIdx].Cells[i].Value = sArr[i];
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
                sbPanel2.Text = openFileDialog1.SafeFileName;   // 파일 이름만 불러옴
                sbPanel2.BackColor = Color.LightGreen;

            }
            catch(SqlException e1)
            {
                MessageBox.Show(e1.Message);
                sbPanel1.Text = "DB cannot open.";
                sbPanel2.BackColor = Color.Red;
            }
        }
        public static string GetToken(int index, char deli, string str)
        {
            string[] Strs = str.Split(deli);          // 구분자: '\'
            string ret = Strs[index];
            return ret;
        }

        string TableName;   //다른 메뉴에서 사용할 DB Table 이름. 현재 open된 테이블
        int RunSql(string s1)
        {
            try
            {   // ex) select * from fStatus
                string sql = s1.Trim();
                sqlCmd.CommandText = sql;   // insert into fstatus values (1,2,3,4)
                if (GetToken(0, ' ', sql).ToUpper() == "SELECT")
                {
                    SqlDataReader sr = sqlCmd.ExecuteReader();

                    TableName = GetToken(3, ' ', sql);
                    dataGrid.Rows.Clear(); //초기화 과정
                    dataGrid.Columns.Clear();

                    for (int i = 0; i < sr.FieldCount; i++) //  Header 처리
                    {
                        string ss = sr.GetName(i);
                        dataGrid.Columns.Add(ss, ss);
                    }
                    for (int i = 0; sr.Read(); i++)     // 1 record read : 1줄
                    {
                        int rIdx = dataGrid.Rows.Add();                
                        for (int j = 0; j < sr.FieldCount; j++)
                        {
                            object str = sr.GetValue(j);
                            dataGrid.Rows[rIdx].Cells[j].Value = str;  
                        }
                    }
                    sr.Close();

                    //for (int i = 0; sr.Read(); i++)     // 1 record read : 1줄
                    //{
                    //    string buf = "";
                    //    for (int j = 0; j < sr.FieldCount; j++)
                    //    {
                    //        object str = sr.GetValue(j);
                    //        buf += $"  {str}";
                    //    }
                    //    tbSql.Text += $"\r\n{buf}";
                    //}
                    //sr.Close();
                }
                else
                {
                    sqlCmd.ExecuteNonQuery();   // select 문 제외 -> no return value
                                                // update, insert, delete, create, alt
                }
                sbPanel1.Text = "Success.";
                sbPanel1.BackColor = Color.LightGreen;
            }
            catch (SqlException e1)
            {
                MessageBox.Show(e1.Message);
                sbPanel1.Text = "Error.";
                sbPanel1.BackColor = Color.Red;
            }
            catch(InvalidOperationException e2)
            {
                MessageBox.Show(e2.Message);
                sbPanel1.Text = "Error.";
                sbPanel1.BackColor = Color.Red;
            }
            return 0;
        }
        private void mnuExecSql_Click(object sender, EventArgs e)
        {
            RunSql(tbSql.Text);
            //dataGrid.Columns = 
        }

        private void mnuSelSql_Click(object sender, EventArgs e)
        {
            RunSql(tbSql.SelectedText);
            //string sql = tbSql.SelectedText;
            //sqlCmd.CommandText = sql;
            //sqlCmd.ExecuteNonQuery();
        }

        private void tbSql_KeyDown(object sender, KeyEventArgs e)
        {
            if (e.KeyCode != Keys.Enter) return;

            string str = tbSql.Text;
            string[] sArr = str.Split('\n');    // 줄바꿈 문자 : \r\n
            int n = sArr.Length;
            string sql = sArr[n - 1].Trim();
            RunSql(sql);
        }

        private void dataGrid_CellBeginEdit(object sender, DataGridViewCellCancelEventArgs e)
        {
            dataGrid.Rows[e.RowIndex].Cells[e.ColumnIndex].ToolTipText = ".";            
        }

        private void mnuUpdate_Click(object sender, EventArgs e)
        {
            for(int i=0; i<dataGrid.Rows.Count; i++)
            {
                for(int j=0; j<dataGrid.Columns.Count; j++)
                {
                    string s = dataGrid.Rows[i].Cells[j].ToolTipText;
                    if (s == ".")   
// update [Table] set [field]=(CellText) where [KeyName]=(Key.CellText)
// update [fStatus] set[Temp]=(10)       where      [ID]=(6)

                    {
                        string tn = TableName;
                        string fn = dataGrid.Columns[j].HeaderText;
                        string ct = (string)dataGrid.Rows[i].Cells[j].Value;
                        string kn = dataGrid.Columns[0].HeaderText;
                        int kt = (int)dataGrid.Rows[i].Cells[0].Value;
                        string sql = $"update {tn} set {fn}={ct} where {kn}={kt}";
                        RunSql(sql);
                    }
                }
            }
        }
    }
}
```
