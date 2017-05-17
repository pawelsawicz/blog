+++
date = "2013-08-26T23:24:02+01:00"
description = "New blogpost for today"
draft = false
title = "Grid in Windows Phone 8 #windowsphone"
weight = 20

+++

When we are creating Windows Phone application first thing that we will be using is GridWindows Phone Grid

Grid, it's our base for creating our world in devices. In this post I want to approach closer this topic. First thought is, how to use it ? and why it's so powerful ?

Let's me answer. It's quite powerful mechanism because <Grid> can be our box for others items like (list box, text block etc.).

</Grid>   
     <TextBlock Text="Some Text"/>
</Grid>

Moreover you can nest <Grid> inside <ListBox> or similar items.

<ListBox x:Name="SpeakerListBox" />
 <Grid>

 </Grid>
</ListBox>

<Grid> has got various members, for declaring rows and columns we have to use  <Grid.RowDefinitions> and <Grid.ColumnDefinitions> by these members we can declare width of column, and height of row.

Example implementation :

<Grid x:Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="50" />
                <RowDefinition Height="*"/>
                <RowDefinition Height="200" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="100" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="50" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
</Grid>

wp_grid_col_row_def

As you can see I made three rows, and four columns. Now if you want place something on specific column or row, it's similar to game "Battleship". Let's place text box, with help comes Grid.Row and Grid.Column.

<TextBox Grid.Row="0" Grid.Column="1" />

wp_grid_col_row_def_box

Notice: good practice - you should implement whole sizes at this step (by Row-Column Definitions), because in my opinion using fixed width and height at items property is mistake (of course sometimes you have to use item property), it's like clothes, you want them to well fit on your body, you do not resize your body to clothes.

Notice: When you are designing your UI by the <Grid> make sure that you are include margin inside these definitions, I think it's good practices to define spacing by this mechanism.

Nesting

You can define <Grid> inside of <ListBox> (i.e), and you can form <ListBox> view as you wish, it's very helpful to create custom view of listbox.

 <Grid Grid.Row="2">
            <Grid.RowDefinitions>
                <RowDefinition Height="50"/>
                <RowDefinition Height="*"/>                
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="24"/>
                <ColumnDefinition Width="*"/>
                <ColumnDefinition Width="24"/>
            </Grid.ColumnDefinitions>
            <TextBox Grid.Row="0" Grid.Column="1"/>
            <ListBox Grid.Row="1" Grid.Column="1">
                <ListBoxItem>
                    <Grid>
                        <Grid.RowDefinitions>
                            <RowDefinition Height="100" />
                            <RowDefinition Height="12"/>
                            <RowDefinition Height="150"/>
                            <RowDefinition Height="12"/>
                            <RowDefinition Height="80"/>
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="100"/>
                            <ColumnDefinition Width="12"/>
                            <ColumnDefinition Width="*"/>
                        </Grid.ColumnDefinitions>
                        <Image Grid.Row="0" Grid.Column="0" Source="/Assets/Images/Bill-Gates.jpg"/>
                        <TextBlock Grid.Row="0" Grid.Column="2" Text="Bill Gates MS CEO" VerticalAlignment="Center" HorizontalAlignment="Left" />
                        <ListBox Grid.Row="2" Grid.ColumnSpan="3">
                            <ListBoxItem>
                                <TextBlock Text="Aenean ac ipsum quam. Nunc condimentum mauris justo, nec interdum erat adipiscing nec. Praesent lacinia est ac sem scelerisque, tincidunt tincidunt nisi ullamcorper. Nam neque erat, gravida ut quam non, vestibulum tempor diam. Duis ut enim diam. Sed nulla augue, tempor eget suscipit ut, cursus sit amet nisi. Integer vel fringilla velit. Suspendisse suscipit metus in nisi viverra commodo. Morbi venenatis interdum tortor eget hendrerit. Interdum et malesuada fames ac ante ipsum primis in faucibus. " TextWrapping="Wrap"/>
                            </ListBoxItem>
                        </ListBox>
                        <Button Grid.Row="4" Grid.ColumnSpan="3" Content="Click me"/>
                    </Grid>
                </ListBoxItem>
            </ListBox>
        </Grid>

wp_listbox_grid

As you can see I didn't use here fixed height, width. Even margins are implemented inside Row-Column Definitions. Of course this grid we can use inside DataTemplate, so if we have dynamic render data.

<ListBox.ItemTemplate>
  <DataTemplate>
    <Grid>
    </Grid>
  </DataTemplate>
</ListBox.ItemTemplate>

Conclusion : When you are modeling UI remember to use grid, and try to not use margin,height,width properties. It's much more maintainable.