
Nama : Muhammad Fathur Rizqi
NIM  : 19.11.2757

# Promos
adalah sebuah aplikasi simulasi pembelian yang menggunakan perhitungan dengan  voucher diskon dan beberapa fitur lainnya untuk user.

## Scope and Functionalities
- User dapat melihat daftar makanan yang ditawarkan
- User dapat memasukkan atau menghapus makanan pilihan ke dalam keranjang
- User dapat melihat subtotal makanan yang terdapat pada keranjang
- User dapat melihat daftar voucher yang ditawarkan
- User dapat menggunakan salah satu voucher
- User dapat melihat harga total termasuk potongannya

## How Does it works?

Pertama user harus memilih item yang ingin dipilih, item-item makanan tersebut berada pada `Penawaran.cs`
```csharp
private void generateContentPenawaran()
        {
            Item coffeLate = new Item("Coffe Late", 30000);
            Item blackTea = new Item("BlackTea", 20000);
            Item milkShake = new Item("Milk Shake", 15000);
            Item watermelonJuice = new Item("Watermelon Juice", 25000);
            Item lemonSquash = new Item("Lemon Squash", 30000);
            Item pizza = new Item("Pizza", 75000);
            Item friedRice = new Item("Fried Rice Special", 45000);

            Penawarancontroller.addItem(coffeLate);
            Penawarancontroller.addItem(blackTea);
            Penawarancontroller.addItem(milkShake);
            Penawarancontroller.addItem(watermelonJuice);
            Penawarancontroller.addItem(lemonSquash);
            Penawarancontroller.addItem(pizza);
            Penawarancontroller.addItem(friedRice);

            listPenawaran.Items.Refresh();
        }
```
kemudian untuk memilih voucher maka dibuatkan class `VoucherList.xaml.cs` yg berada didalam `VoucherList.xaml` kemudian setiap kupon diberi potongan harga nya masing-masing dan perhitungannya
terdapat pada `KeranjangBelanja.cs` seperti pada kode dibawah ini
``` csharp
  private void calculateSubTotal()
        {
            double subtotal = 0;
            double potongan = 0;
            foreach (Item item in itemBelanja)
            {
                subtotal += item.price;
            }

            foreach (Voucher voucher in itemVoucher)
            {
                if (voucher.discInPercent != 0)
                {

                    if(voucher.discInPercent == 30)
                    {
                        if(subtotal >= 100000)
                        {
                            potongan -= 30000;
                        } else
                        {
                            potongan -= subtotal * (voucher.discInPercent / 100);
                        }
                    } else { 
                        potongan -= subtotal * (voucher.discInPercent/100);
                    }
                }

                if(voucher.disc != 0)
                {
                    potongan -= voucher.disc;
                }
            }
            payment.updateTotal(subtotal, potongan); 
        }
    }
```
dan untuk menghapus item yang telah dipilih terdapat pada function di `MainWindow.xaml.cs

```csharp
 private void listBoxPesanan_ItemClicked(object sender, MouseButtonEventArgs e)
        {
            if (MessageBox.Show("Kamu ingin menghapus item ini?",
                    "Konfirmasi", MessageBoxButton.YesNo) == MessageBoxResult.Yes)
            {
                ListBox listBox = sender as ListBox;
                Item item = listBox.SelectedItem as Item;
                controller.deleteSelectedItem(item);
            }
        }
``` 
