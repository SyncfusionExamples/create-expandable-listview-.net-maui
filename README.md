**[View document in Syncfusion .NET MAUI Knowledge Base](https://www.syncfusion.com/kb/13078/how-to-create-an-expandable-listview-sflistview-in-net-maui)**

## Sample

```xaml
<ContentPage.Resources>
    <local:Converter x:Name="Converter" x:Key="Converter"/>
</ContentPage.Resources>

<ListView:SfListView x:Name="listView" ItemsSource="{Binding ContactsInfo}"
                    AutoFitMode="DynamicHeight" SelectionMode="None" ItemSpacing="0,2.5,0,2.5">
    <ListView:SfListView.ItemTemplate>
        <DataTemplate>
            <Grid x:Name="grid" Padding="1" Margin="1" RowSpacing="0">
                <Grid.RowDefinitions>
                    <RowDefinition Height="60" />
                    <RowDefinition Height ="*"/>
                </Grid.RowDefinitions>
                <Grid RowSpacing="0">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="60" />
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="50" />
                    </Grid.ColumnDefinitions>
                    <code>
                    . . .
                    . . .
                    <code>
                </Grid>
                
                <Grid x:Name="ExpandGrid" IsVisible="{Binding IsVisible}" ColumnSpacing="0" RowSpacing="0" Grid.Row="1">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="1" />
                        <RowDefinition Height="40" />
                        <RowDefinition Height="40" />
                        <RowDefinition Height="40" />
                        <RowDefinition Height="40" />
                        <RowDefinition Height="40" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="50" />
                        <ColumnDefinition Width="*" />
                    </Grid.ColumnDefinitions>
                    <code>
                    . . .
                    . . .
                    <code>
                </Grid>
            </Grid>
        </DataTemplate>
    </ListView:SfListView.ItemTemplate>
</ListView:SfListView>

C#:
listview.ItemTapped += ListView_ItemTapped;

private void ListView_ItemTapped(object sender, Syncfusion.Maui.ListView.ItemTappedEventArgs e)
{
    if (tappedItem != null && tappedItem.IsVisible)
    {
        var previousIndex = listview.DataSource.DisplayItems.IndexOf(tappedItem);

        tappedItem.IsVisible = false;

        if (DeviceInfo.Platform != DevicePlatform.MacCatalyst)
            listview.RefreshItem(previousIndex, previousIndex, false);
    }

    if (tappedItem == (e.DataItem as Contact))
    {
        if (DeviceInfo.Platform == DevicePlatform.MacCatalyst)
        {
            var previousIndex = listview.DataSource.DisplayItems.IndexOf(tappedItem);
            listview.RefreshItem(previousIndex, previousIndex, false); 
        }

        tappedItem = null;
        return;
    }

    tappedItem = e.DataItem as Contact;
    tappedItem.IsVisible = true;

    if (DeviceInfo.Platform == DevicePlatform.MacCatalyst)
    {
        var visibleLines = this.listview.GetVisualContainer().ScrollRows.GetVisibleLines();
        var firstIndex = visibleLines[visibleLines.FirstBodyVisibleIndex].LineIndex;
        var lastIndex = visibleLines[visibleLines.LastBodyVisibleIndex].LineIndex;
        listview.RefreshItem(firstIndex, lastIndex, false);
    }
    else
    {
        var currentIndex = listview.DataSource.DisplayItems.IndexOf(e.DataItem);
            listview.RefreshItem(currentIndex, currentIndex, false);
    }
}

public class Converter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? 100 : 0;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```
