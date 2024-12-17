Akun login = username: asan,
             password: asanmaster

default password setelah buat akun user baru = password

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
//

Default JSON parameter untuk procedure save
{UserID:'$USERNAME'}


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
//            
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
