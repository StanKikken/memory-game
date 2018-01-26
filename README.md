# memory-game


source code


using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace memory_spel
{
    public partial class Form1 : Form
    {
        Label firstClicked = null;

        Label secondClicked = null;

        Random random = new Random();


        List<string> icons = new List<string>()
        {
            "#", "#", "V", "V", "a", "a", "k", "k",
            "l", "l", "v", "v", "p", "p", "X", "X"
        };

        private void AssignIconsToSquares()
        {

            foreach (Control control in tableLayoutPanel1.Controls)
            {

                Label iconLabel = control as Label;
                if (iconLabel != null)
                {
                    int randomNumber = random.Next(icons.Count);
                    iconLabel.Text = icons[randomNumber];
                    icons.RemoveAt(randomNumber);
                    iconLabel.ForeColor = iconLabel.BackColor;
                }
            }
        }


        public Form1()
        {
            InitializeComponent();

            AssignIconsToSquares();
        }

        //klik event

        //als je klikt wordt de kleur van het icoon veranderd van de achtergrondkleur naar zwart
        private void label_Click(object sender, EventArgs e)
        {
            if (timer1.Enabled == true)
                return;

            Label clickedLabel = sender as Label;

            if (clickedLabel != null)
            {

                if (clickedLabel.ForeColor == Color.Black)
                    return;


            }

            if (firstClicked == null)
            {
                firstClicked = clickedLabel;
                firstClicked.ForeColor = Color.Black;

                return;
            }
            secondClicked = clickedLabel;
            secondClicked.ForeColor = Color.Black;

            CheckForWinner();

            if (firstClicked.Text == secondClicked.Text)
            {
                firstClicked = null;
                secondClicked = null;
                return;
            }
            //als je twee verschillende icoontjes aanklikt start de timer en blijfen de icoontjes nog 0,75 sec staan
            //daarna krijgen ze weer de achtergrondkleur
            timer1.Start();
        }

        //timer van 0,75 sec
        private void timer1_Tick(object sender, EventArgs e)
        {

            timer1.Stop();

            firstClicked.ForeColor = firstClicked.BackColor;
            secondClicked.ForeColor = secondClicked.BackColor;

            firstClicked = null;
            secondClicked = null;
        }

        //checken of iemand gewonnen heeft
        private void CheckForWinner()
        {
            foreach (Control control in tableLayoutPanel1.Controls)
            {
                Label iconLabel = control as Label;

                if (iconLabel != null)
                {
                    if (iconLabel.ForeColor == iconLabel.BackColor)
                        return;
                }
            }
            //Confirmatie dat je gewonnen hebt
            MessageBox.Show("hihi! je hebt mijn game gewonnen!");
            Close();
        }

        
    }
}
