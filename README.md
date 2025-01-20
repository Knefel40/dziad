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
