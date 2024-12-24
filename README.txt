Akun login = username: asan,
             password: asanmaster

default password setelah buat akun user baru = password

Default JSON parameter untuk procedure save
{UserID:'$USERNAME'}

// SQL Create Karyawan 

CREATE TABLE karyawan (
    karyawan_id INT PRIMARY KEY ,
    nama NVARCHAR(100),
    jenis_kelamin NVARCHAR(100),
    tanggal_lahir DATE,
    Status TINYINT,
    IsDeleted TINYINT,
    CreatedBy VARCHAR(32),
    CreatedDate DATETIME,
    LastUpdatedBy VARCHAR(50),
    LastUpdatedDate DATETIME
);
//


// Query untuk buat Vendor CBO combo box 


CREATE PROCEDURE sp_usrdef_vendor_cbo
    @Search varchar(100) = ''
AS 
BEGIN
    SELECT Vendor_ID as 'value', Vendor_Name as 'text' FROM vendors  
END 
GO
// setelah dibuat tambahkan lagi ke function definition fc_vendor_cbo


// Sample combobox datasource

HTML

  <div class="form-group"> 
     <label for="Vendor_ID" class="col-sm-2 col-xs-12 control-label">{{formLabel.Vendor_ID}}</label> 
     <div class="col-sm-4 col-xs-12"> 
         <input name="Vendor_ID" kendo-combo-box k-options="cboVendor" ng-model="data.Vendor_ID" type="number" xt-validate class="form-control" id="Vendor_ID"  step = "any" > 
     </div> 
 </div> 
//

JS

   $scope.cboVendor = {
      placeholder: "Pilih Vendor",
      dataTextField: "text",
      dataValueField: "value",
      filter: "contains",
      autoBind: true,
      valuePrimitive: true,
      dataSource: {
          type: "jsonp",
          transport: {
              read: {
                  url: baseAddress + "appsystem/config/api?fnid=fc_vendor_cbo",
              }
          },
          schema: {
              data: 'data.Table',
          }
      }
  }
           
// Radio Button 

HTML

 <div class="form-group"> 
     <label for="JENIS_KELAMIN" class="col-sm-2 col-xs-12 control-label">{{formLabel.JENIS_KELAMIN}} <b style="color:red;font-size:20px">*</b></label> 
     <div class="col-sm-3 col-xs-12"> 
        <section>
        <div class="inline-group">
        <label class="radio">
        <input type="radio" ng-model="data.JENIS_KELAMIN" value="1" name="RadioJK">
        <i></i>Pria</label>
        <label class="radio">
        <input type="radio" ng-model="data.JENIS_KELAMIN"  value="0" name="RadioJK">
        <i></i>Wanita</label>
        </div>
        </section>
     </div> 
  </div>


// kendo-date-picker HTML

  <div class="form-group">
      <label for="CreateDate" ng-show="isExpired" class="col-sm-2 col-xs-12 control-label">Create Date</label>
      <div class="col-sm-3 col-xs-12">
          <input name="CreateDate"  ng-show="isExpired" ng-model="dataDoc.DocDate2"   options="datePickerConfig" kendo-date-picker   type="datetime" xt-validate class="form-control" id="CreateDate">
      </div>
  </div>



// status combo box tanpa get api
HTML
  <div class="form-group"> 
     <label for="status" class="col-sm-2 col-xs-12 control-label">{{formLabel.status}}</label> 
     <div class="col-sm-4 col-xs-12"> 
         <input name="status_pembayaran" kendo-combo-box k-options="cboStatus" ng-model="data.status-pembayaran" type="text" xt-validate class="form-control" id="status"  step = "any" > 
     </div> 
 </div> 
JS
  $scope.cboStatus = {
      placeholder: "Pilih Status",
      dataTextField: "text",
      dataValueField: "value",
      filter: "contains",
      autoBind: true,
      valuePrimitive: true,
     dataSource: [
            { text: "Dibayar", value: "Di Bayar" },
            { text: "Belom Bayar", value: "Belom Bayar" }
        ]
  }

// combobox penggabungan value dan id saya membuat api fc_employee_cbo khusus get id dan nama employee lalu daftarkan ke function definition aga bisa dipanggil
$scope.cboEmployee = {
    placeholder: "Pilih Employee ",
    dataTextField: "ID_Name",
    dataValueField: "value",
    filter: "contains",
    autoBind: true,
    valuePrimitive: true,
    dataSource: {
        type: "jsonp",
        transport: {
            read: {
                url: baseAddress + "appsystem/config/api?fnid=fc_employee_cbo",
            }
        },
        schema: {
           data: function(response) {
              // Tambahkan properti `ID_Name` tanpa mengubah struktur asli
              response.data.Table.forEach(item => {
                  item.ID_Name = `${item.text} - Employe_ID(${item.value})`; // Gabungkan nama dan ID
              });
              return response.data.Table; // Tetap kembalikan `data.Table`
          }
        }
    }
}



// contoh join table agar yang muncul name bukan id dan bisa di cari di kolom search berdasarkan where yang diinputkan dan diterima di @search

Create PROCEDURE sp_usrdef_asset_list
    @Search varchar(100) = ''
AS 
BEGIN
    SELECT 
        Asset.*, 
        Product.Product_Name, 
        Location.Location_Name, 
        Asset_Type.Asset_Type_Name 
    FROM 
        Asset  
    JOIN 
        Product ON Asset.Product_ID = Product.Product_ID
    JOIN 
        Location ON Asset.Location_ID = Location.Location_ID
    JOIN 
        Asset_Type ON Asset.Asset_Type_ID = Asset_Type.Asset_Type_ID
    WHERE 
        (@Search = '' OR Asset.Asset_Name LIKE '%' + @Search + '%') OR
        (@Search = '' OR Asset.Asset_Status LIKE '%' + @Search + '%') OR
        (@Search = '' OR Product.Product_Name LIKE '%' + @Search + '%') OR
        (@Search = '' OR Location.Location_Name LIKE '%' + @Search + '%') OR
        (@Search = '' OR Asset_Type.Asset_Type_Name LIKE '%' + @Search + '%')
END
