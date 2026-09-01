# How to create an expandable ListView (SfListView) in .NET MAUI ?

The [.NET MAUI ListView (SfListView)](https://www.syncfusion.com/maui-controls/maui-listview) allows you to expand the items at run time by setting the [AutoFitMode](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.ListView.SfListView.html#Syncfusion_Maui_ListView_SfListView_AutoFitMode) property to DynamicHeight. By manipulating a model property, you can control the visibility of additional views within the [ItemTemplate](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.ListView.SfListView.html#Syncfusion_Maui_ListView_SfListView_ItemTemplate). Define a ListView with expandable items by using a Grid in the ItemTemplate, where an additional view is displayed based on a boolean model property.

XAML

Define a ListView with expandable items by using a Grid in the ItemTemplate, where an additional view is displayed based on a boolean model property.

<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ExpandableListView.ExpandableListViewPage"
                 Title="Expandable ListView"
                 xmlns:syncfusion="clr-namespace:Syncfusion.Maui.ListView;assembly=Syncfusion.Maui.ListView"
                 BackgroundColor="White">
        <ContentPage.Content>
            <ListView:SfListView x:Name="listView"
                                 ItemsSource="{Binding ContactsInfo}"
                                 AutoFitMode="DynamicHeight"
                                 SelectionMode="None"
                                 ItemSpacing="0,2.5,0,2.5">
                <ListView:SfListView.ItemTemplate>
                    <DataTemplate>
                        <Grid x:Name="grid" VerticalOptions="FillAndExpand" BackgroundColor="White" HorizontalOptions="FillAndExpand" RowSpacing="0">
                            <Grid.RowDefinitions>
                                <RowDefinition Height="60" />
                                <RowDefinition Height="*" />
                            </Grid.RowDefinitions>
                            <Grid RowSpacing="0" Grid.Row="0">
                                <!-- Main View Content -->
                                <Label Text="{Binding ContactName}" VerticalOptions="Center" />
                            </Grid>
                            
                            <!-- Expandable view -->
                            <Grid x:Name="ExpandGrid" IsVisible="{Binding IsVisible}" ColumnSpacing="0" RowSpacing="0" Grid.Row="1" Padding="10,0,0,0"
                                  HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand">
                                <!-- Expanded View Content -->
                                <Label Text="{Binding ContactDetails}" />
                            </Grid>
                        </Grid>
                    </DataTemplate>
                </ListView:SfListView.ItemTemplate>
            </ListView:SfListView>
        </ContentPage.Content>
    </ContentPage>
C#

In the [SfListView.ItemTapped](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.ListView.SfListView.html#Syncfusion_Maui_ListView_SfListView_ItemTapped) event, update the IsVisible property. For the MacCatalyst platform, refresh the item by using the [SfListView.RefreshItem](https://help.syncfusion.com/cr/maui/Syncfusion.Maui.ListView.SfListView.html#Syncfusion_Maui_ListView_SfListView_RefreshItem_System_Int32_System_Int32_System_Boolean_) method.

private void ListView_ItemTapped(object sender, Syncfusion.Maui.ListView.ItemTappedEventArgs e)
    {
        if (tappedItem != null && tappedItem.IsVisible)
        {
            var previousIndex = listview.DataSource.DisplayItems.IndexOf(tappedItem);
     
            tappedItem.IsVisible = false;
     
            if (Device.RuntimePlatform != Device.MacCatalyst)
                Device.BeginInvokeOnMainThread(() => { listview.RefreshItem(previousIndex, previousIndex, false); });
        }
     
        if (tappedItem == (e.ItemData as Contact))
        {
            if (Device.RuntimePlatform == Device.MacCatalyst)
            {
                var previousIndex = listview.DataSource.DisplayItems.IndexOf(tappedItem);
                Device.BeginInvokeOnMainThread(() => { listview.RefreshItem(previousIndex, previousIndex, false); });
            }
     
            tappedItem = null;
            return;
        }
     
        tappedItem = e.ItemData as Contact;
        tappedItem.IsVisible = true;
     
        if (Device.RuntimePlatform == Device.MacCatalyst)
        {
            var visibleLines = this.listview.GetVisualContainer().ScrollRows.GetVisibleLines();
            var firstIndex = visibleLines[visibleLines.FirstBodyVisibleIndex].LineIndex;
            var lastIndex = visibleLines[visibleLines.LastBodyVisibleIndex].LineIndex;
            Device.BeginInvokeOnMainThread(() => { listview.RefreshItem(firstIndex, lastIndex, false); });
        }
        else
        {
            var currentIndex = listview.DataSource.DisplayItems.IndexOf(e.ItemData);
            Device.BeginInvokeOnMainThread(() => { listview.RefreshItem(currentIndex, currentIndex, false); });
        }
    }
    

![Expandable ListView in .Net MAUI](https://www.syncfusion.com/uploads/user/kb/maui/maui-1504/maui-1504_img1.png)

**Demo**
Download the complete sample on [GitHub](https://github.com/SyncfusionExamples/create-expandable-listview-.net-maui).

**Conclusion**

I hope you enjoyed learning how to create an expandable ListView in .NET MAUI.

You can refer to our [.NET MAUI ListView feature tour](https://www.syncfusion.com/maui-controls/maui-listview) page to learn about its other groundbreaking feature representations and [documentation](https://help.syncfusion.com/maui/listview/getting-started), and how to quickly get started with configuration specifications. Explore our [.NET MAUI ListView example](https://github.com/syncfusion/maui-demos/tree/master/MAUI/ListView) to understand how to create and manipulate data.

For current customers, check out our components from the [License and Downloads](https://www.syncfusion.com/sales/teamlicense) page. If you are new to Syncfusion®, try our 30-day [free trial](https://www.syncfusion.com/downloads/maui) to check out our other controls.

Please let us know in the comments section if you have any queries or require clarification. Contact us through our [support forums](https://www.syncfusion.com/forums), [Direct-Trac](https://support.syncfusion.com/create), or [feedback portal](https://www.syncfusion.com/feedback/maui?control=sflistview). We are always happy to assist you!
