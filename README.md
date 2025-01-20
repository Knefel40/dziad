<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApp1"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <DockPanel>
        <Menu DockPanel.Dock="Top">
            <MenuItem Header="Plik">
                <MenuItem Header="Odczyt" Click="OdczytajZPliku">
                </MenuItem>

                <MenuItem Header="Zapis" Click="ZapiszDoPliku"></MenuItem>
                <MenuItem Header="Zamknij"></MenuItem>
            </MenuItem>

            <MenuItem Header="Znajdz">
                <MenuItem Header="Liczby pierwsze" Click="SzukajLiczbyPierwsze">
                </MenuItem>

                <MenuItem Header="Max" Click="PodajMaksymalnaLiczbe"></MenuItem>
                <MenuItem Header="Min" Click="PodajMinimalnaLiczbe"></MenuItem>
                <MenuItem Header="Liczba wystąpień" Click="PodajLiczbeWystapien"></MenuItem>
            </MenuItem>
        </Menu>
        <TextBlock DockPanel.Dock="Bottom">Wynik: </TextBlock>

    </DockPanel>
</Window>








<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApp1"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <DockPanel>
        <Menu DockPanel.Dock="Top">
            <MenuItem Header="Plik">
                <MenuItem Header="Odczyt" Click="OdczytajZPliku">
                </MenuItem>
                <MenuItem Header="Zapis" Click="ZapiszDoPliku"></MenuItem>
                <MenuItem Header="Zamknij" Click="Zamknij"></MenuItem>
            </MenuItem>

            <MenuItem Header="Znajdz">
                <MenuItem Header="Liczby pierwsze" Click="SzukajLiczbyPierwsze"></MenuItem>
                <MenuItem Header="Max" Click="PodajMaksymalnaLiczbe"></MenuItem>
                <MenuItem Header="Min" Click="PodajMinimalnaLiczbe"></MenuItem>
                <MenuItem Header="Liczba wystąpień" Click="PodajLiczbeWystapien"></MenuItem>
            </MenuItem>
        </Menu>
        <TextBlock Name="WynikTextBlock" DockPanel.Dock="Bottom" TextWrapping="Wrap" Margin="10">Wynik: </TextBlock>
    </DockPanel>
</Window>






using Microsoft.Win32;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Windows;

namespace WpfApp1
{
    public partial class MainWindow : Window
    {
        private List<int> liczby = new List<int>();

        public MainWindow()
        {
            InitializeComponent();
        }

        private void OdczytajZPliku(object sender, RoutedEventArgs e)
        {
            OpenFileDialog openFileDialog = new OpenFileDialog
            {
                Filter = "Pliki tekstowe (*.txt)|*.txt|Wszystkie pliki (*.*)|*.*"
            };

            if (openFileDialog.ShowDialog() == true)
            {
                try
                {
                    var lines = File.ReadAllLines(openFileDialog.FileName);
                    liczby = lines.SelectMany(line => line.Split(new[] { ' ', ',', ';', '\n', '\t' }, StringSplitOptions.RemoveEmptyEntries))
                                  .Where(x => int.TryParse(x, out _))
                                  .Select(int.Parse)
                                  .ToList();
                    WynikTextBlock.Text += "\nPlik został odczytany.";
                }
                catch (Exception ex)
                {
                    MessageBox.Show($"Błąd podczas odczytu pliku: {ex.Message}");
                }
            }
        }

        private void ZapiszDoPliku(object sender, RoutedEventArgs e)
        {
            SaveFileDialog saveFileDialog = new SaveFileDialog
            {
                Filter = "Pliki tekstowe (*.txt)|*.txt|Wszystkie pliki (*.*)|*.*"
            };

            if (saveFileDialog.ShowDialog() == true)
            {
                try
                {
                    File.WriteAllLines(saveFileDialog.FileName, liczby.Select(x => x.ToString()));
                    WynikTextBlock.Text += "\nDane zapisano do pliku.";
                }
                catch (Exception ex)
                {
                    MessageBox.Show($"Błąd podczas zapisu pliku: {ex.Message}");
                }
            }
        }

        private void SzukajLiczbyPierwsze(object sender, RoutedEventArgs e)
        {
            var liczbyPierwsze = liczby.Where(IsPrime).ToList();
            WynikTextBlock.Text += $"\nLiczby pierwsze: {string.Join(", ", liczbyPierwsze)}";
        }

        private void PodajMaksymalnaLiczbe(object sender, RoutedEventArgs e)
        {
            if (liczby.Any())
            {
                int max = liczby.Max();
                WynikTextBlock.Text += $"\nMaksymalna liczba: {max}";
            }
            else
            {
                WynikTextBlock.Text += "\nBrak danych.";
            }
        }

        private void PodajMinimalnaLiczbe(object sender, RoutedEventArgs e)
        {
            if (liczby.Any())
            {
                int min = liczby.Min();
                WynikTextBlock.Text += $"\nMinimalna liczba: {min}";
            }
            else
            {
                WynikTextBlock.Text += "\nBrak danych.";
            }
        }

        private void PodajLiczbeWystapien(object sender, RoutedEventArgs e)
        {
            var dialog = new LiczbaDialog();
            if (dialog.ShowDialog() == true)
            {
                int liczba = dialog.PodanaLiczba;
                int wystapienia = liczby.Count(x => x == liczba);
                WynikTextBlock.Text += $"\nLiczba {liczba} występuje {wystapienia} razy.";
            }
        }

        private void Zamknij(object sender, RoutedEventArgs e)
        {
            Application.Current.Shutdown();
        }

        private bool IsPrime(int number)
        {
            if (number < 2) return false;
            for (int i = 2; i <= Math.Sqrt(number); i++)
            {
                if (number % i == 0) return false;
            }
            return true;
        }
    }
}







<Window x:Class="WpfApp1.LiczbaDialog"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Podaj liczbę" Height="150" Width="300">
    <StackPanel Margin="10">
        <TextBlock>Podaj liczbę:</TextBlock>
        <TextBox Name="LiczbaTextBox" Margin="0,5,0,10"></TextBox>
        <Button Content="OK" Width="75" HorizontalAlignment="Right" Click="OK_Click"></Button>
    </StackPanel>
</Window>







using System.Windows;

namespace WpfApp1
{
    public partial class LiczbaDialog : Window
    {
        public int PodanaLiczba { get; private set; }

        public LiczbaDialog()
        {
            InitializeComponent();
        }

        private void OK_Click(object sender, RoutedEventArgs e)
        {
            if (int.TryParse(LiczbaTextBox.Text, out int liczba))
            {
                PodanaLiczba = liczba;
                DialogResult = true;
            }
            else
            {
                MessageBox.Show("Podaj prawidłową liczbę!");
            }
        }
    }
}













using Microsoft.Win32;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Windows;

namespace WpfApp1
{
    public partial class MainWindow : Window
    {
        private List<int> liczby = new List<int>();

        public MainWindow()
        {
            InitializeComponent();
        }

        private void OdczytajZPliku(object sender, RoutedEventArgs e)
        {
            OpenFileDialog openFileDialog = new OpenFileDialog();
                openFileDialog.Filter = "plik tekstowy | *.txt";

            if (openFileDialog.ShowDialog() == true)
            {
                    var lines = File.ReadAllLines(openFileDialog.FileName);
                    liczby = lines.SelectMany(line => line.Split(new[] { ' ', ',', ';', '\n', '\t' }, StringSplitOptions.RemoveEmptyEntries))
                                  .Where(x => int.TryParse(x, out _))
                                  .Select(int.Parse)
                                  .ToList();
            }
        }

        private void ZapiszDoPliku(object sender, RoutedEventArgs e)
        {
            SaveFileDialog saveFileDialog = new SaveFileDialog
            {
                Filter = "Pliki tekstowe (*.txt)|*.txt|Wszystkie pliki (*.*)|*.*"
            };

            if (saveFileDialog.ShowDialog() == true)
            {
                    File.WriteAllLines(saveFileDialog.FileName, liczby.Select(x => x.ToString()));
                    WynikTextBlock.Text += "\nDane zapisano do pliku.";

            }
        }

        private void SzukajLiczbyPierwsze(object sender, RoutedEventArgs e)
        {
            var liczbyPierwsze = liczby.Where(JestPierwsza).ToList();
            WynikTextBlock.Text += $"\nLiczby pierwsze: {string.Join(", ", liczbyPierwsze)}";
        }

        private void PodajMaksymalnaLiczbe(object sender, RoutedEventArgs e)
        {
            if (liczby.Any())
            {
                int max = liczby.Max();
                WynikTextBlock.Text += $"\nMaksymalna liczba: {max}";
            }
            else
            {
                WynikTextBlock.Text += "\nBrak danych.";
            }
        }

        private void PodajMinimalnaLiczbe(object sender, RoutedEventArgs e)
        {
            if (liczby.Any())
            {
                int min = liczby.Min();
                WynikTextBlock.Text += $"\nMinimalna liczba: {min}";
            }
            else
            {
                WynikTextBlock.Text += "\nBrak danych.";
            }
        }

        private void PodajLiczbeWystapien(object sender, RoutedEventArgs e)
        {
            var okno = new LiczbaDialog();
            if (okno.ShowDialog() == true)
            {
                int liczba = okno.PodanaLiczba;
                int wystapienia = liczby.Count(x => x == liczba);
                WynikTextBlock.Text += $"\nLiczba {liczba} występuje {wystapienia} razy.";
            }
        }

        private void Zamknij(object sender, RoutedEventArgs e)
        {
            Application.Current.Shutdown();
        }

        private bool JestPierwsza(int number)
        {
            if (number < 2) return false;
            for (int i = 2; i <= Math.Sqrt(number); i++)
            {
                if (number % i == 0) return false;
            }
            return true;
        }
    }
}
