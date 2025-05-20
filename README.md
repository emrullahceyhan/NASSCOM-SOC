# NASSCOM-SOC

## Section 1 - Design Preparation and Synthesis 

This section explains how to synthesize the picorv32a project using the OpenLane flow and interprets some basic parameters from the synthesis results. In this step of OpenLane flow, Yosys open source synthesizer is used.

```sh
# Change directory to openlane flow directory
> cd Desktop/work/tools/openlane_working_dir/openlane
```
```sh
# The OpenLane flow is in docker so type docker to invoke the flow
> docker
```
We have entered the docker subsystem as shown in below. 

#### Buraya 1.ss i ekle

```sh
# To invoke the OpenLANE flow in the Interactive mode use below command
> ./flow.tcl -interactive
```
The OpenLane flow has now been opened, and the terminal output is as follows.

#### Buraya 2.ss i ekle

```sh
# For the OpenLane flow to function properly, the appropriate package needs to be provided.
> package require openlane 0.9
```
#### Buraya 3.ss i ekle

```sh
# In this study, the picorv32a project will be synthesized, but before that, some necessary files and directories need to be created. Therefore, the design must first be prepared using the prep design step.
> prep -design picorv32a
```
#### Buraya 4.ss i ekle

```sh
# Then run synthesis
> run_synthesis
```
#### Buraya 5.ss i ekle

The synthesis was completed with negative TNS(Total Negative Slack) and WNS(Worse Negative Slack) values.

The synthesis results can be reviewed under the `Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/Day-Month_Hour-Minute` path, specifically in the **reports** and **results** sections.

Within the `reports/synthesis` directory under `Day-Month_Hour-Minute`, many timing details can be accessed. Timing results from all paths can be analyzed.

As a result of the synthesis, resource utilization outputs can be accessed either from the synthesis screen or from the `1-yosys_4.stat.rpt` file located under `reports/synthesis`.

#### Buraya 6.ss i ekle


Above, the resource usage resulting from the synthesis of the project can be seen. From this, a percentage-based utilization calculation can be derived.

For example there are 14876 cells are available in the system and 1613 D type Flip-Flops are used.

Then D Flip-Flop usage percentage = (#D Flip-Flop) / (#Total Cells available)

So about %10.84 of cells are used by D Flip-Flop


## Section 2 - Floorplanning, Placement and Introduction to Library Cells
### Section 2.1 - Floorplanning


```sh
# Change directory to openlane flow directory
> cd Desktop/work/tools/openlane_working_dir/openlane
```
```sh
# The OpenLane flow is in docker so type docker to invoke the flow
> docker
```
```sh
# To invoke the OpenLANE flow in the Interactive mode use below command
> ./flow.tcl -interactive
```

```sh
# For the OpenLane flow to function properly, the appropriate package needs to be provided.
> package require openlane 0.9
```

```sh
# In this study, the picorv32a project will be synthesized, but before that, some necessary files and directories need to be created. Therefore, the design must first be prepared using the prep design step.
> prep -design picorv32a
```

```sh
# Then run synthesis
> run_synthesis
```

```sh
# Then run floorplan
> run_floorplan
```
Below is the example terminal output of floorplannig.
#### Buraya 7.ss i ekle

```sh
Note : There are several files that contain the configurations used during the flow. The priority order among them is as follows:
1. Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/sky130A_sky130A_fd_sc_hd_config.tcl
2. Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/config.tcl
3. Desktop/work/tools/openlane_working_dir/openlane/*.tcl ( Includes defaults of every step in the flow)

Configuration files with a higher priority level always overwrite the ones that come after them.
```

Additionally, the values set for the `FP_IO_VMETAL` and `FP_IO_HMETAL` parameters are always processed as one more than the specified value. For example, if `FP_IO_VMETAL` is set to 3 and `FP_IO_HMETAL` to 4, you will see in the floorplan log files that these values are taken as 4 for `FP_IO_VMETAL` and 5 for `FP_IO_HMETAL`.


If you're curious about how your floorplan result looks, you can check the following file.
`Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/Day-Month_Hour-Minute/results/floorplan/picorv32a.floorplan.def`

Below is the example terminal output of floorplannig.
#### Buraya 8.ss i ekle

Terminalde DIEAREA ( 0 0 )( 660685 671405) satırındaki ilk 0 lover left x, ikinci 0 lover left y,660685 upper rigth x ve 671405 ise upper rigth y değerini göstermektedir. Bu değerlere unit distance denir. Bu değeleri kullanarak çipin alanını hesaplayabilirsiniz. Birim değer bir üst satırda görüldüğü gibi micron cinsindedir ve 1000 unit distance 1 microna eşittir.

> Die width in unit distance = 660685 - 0 = 660685

> Die higth in unit distance = 671405 - 0 = 671405

> Die width in microns = 660685/1000 = 660.685 microns

> Die higth in microns = 671405/1000 = 671.405 microns

> Die area = die width*die higth = 443587.212425 square microns

Floorplan sonrası layoutun nasıl göründüğüne bakmak için;
```sh
# Öncelikte aşağıdaki locationa gidin
> cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/Day-Month_Hour-Minute/results/floorplan/
# Daha sonra aşağıdaki komut ile magic toolunu kullarak layoutu açın
> magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
``` 
Aşağıda floorplan layoutunu görebilirsiniz. Tasarımı ortalamak için önce 's' daha sonra 'v' tuşuna basınız.

#### Buraya 9.ss i ekle


'z' ile zoomin 'Z' ile zoomout yapabilirsiniz. Aşağıda pinoutlardan biri görünmektedir.

#### Buraya 10.ss i ekle

mouseu pinin üzerine tutup 's' ye basarsanız onu seçmiş olursunuz. Ardından `tkcon` ekranından 'what' yazarsanız seçtiğiniz birimin layout ve label bilgisini görebilirsiniz.

#### Buraya 11.ss i ekle

### Section 2.2 - Placement

```sh
# Global Placement by defaults
> run_placement
``` 
placement sonucunun terminal çıktısı aşağıdaki gibidir.
#### Buraya 12.ss i ekle

magic toolunu kullanarak placement sonucuna bakmak için aşağıdaki komutu girin
```sh
# Öncelikte aşağıdaki locationa gidin
> cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/Day-Month_Hour-Minute/results/placement/

# Daha sonra aşağıdaki komut ile magic toolunu kullarak layoutu açın
> magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
``` 
placement sonucu bu şekilde görünecektir.
#### Buraya 13.ss i ekle

belirli bir bölgeye yaklaşınca celllerin legacy placed hali görülmektedir.
#### Buraya 14.ss i ekle

## Section 3 -  Design library cell using Magic Layout and ngspice characterization

İlk aşamada github üzerinden bir cell reposu indirmemiz gerekiyor.
```sh
# Repoyu indriceğimiz komuta gidind
> cd Desktop/work/tools/openlane_working_dir/openlane

# Daha sonra aşağıdaki komut ile ilgili repoyu clonelayın
> git clone https://github.com/nickson-jose/vsdstdcelldesig

# 'vsdstdcelldesign' isminde bir klasör oluşacak. O klasöre gidin
> cd vsdstdcelldesign

# sky130A.tec dosyasına kolay erişebilmek için o dosyayı indirdiğimiz reponun içine aşağıdaki komut ile kopyalayın
> cp ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Ardından magic toolunu kullarak indirdiğimiz inverter layoutu açın
> magic -T sky130A.tech sky130_inv.mag &
``` 
Aşağıdaki resimde custom inverter layoutu görebilrsiniz.

#### Buraya 15.ss i ekle

Layoutun alt kısmında polysliconun kesiştiği alana cursoru getirip 's' ile seçtikten sonra tkcon ekranına 'what' yazınca oranın N-MOS olduğunu görebilirsiniz.

#### Buraya 16.ss i ekle

Ayı işlemi üst kısma uygulayınca P-MOS olduğunu görebilirsiniz.

#### Buraya 17.ss i ekle

Eğer 's' e iki kere tıklarsanız cursorun seçtiği layerin bağlatısı highlighted olacaktır. Aşağıda outputun bağlantısı görülmektedir.

#### Buraya 18.ss i ekle

Layout üzerinde bir hata olduğunda bunu magic toolun üst kısmında DRC bölümünde görebilirsiniz.Aşağıda polysliconun bir kısmını sildik ve DRC=3 yani 3 tane hata olduğunu görebilirsiniz.

#### Buraya 19.ss i ekle

Tasarlanan layoutun işlevselliğini görebilmek için ngspice üzerinden simülasyon yapılabilir. Bunun için öncelikle tasarımı ngspice ın çalıştırabileceği formata dönüştürmemiz lazım. Bunun için aşağıdaki komutları tkcon ekranında çalıştırın.

```sh
# .ext formatında tasarımı extract edin
> extract all

# ext to spice yapmadan önce parasitic extrationsları aktif edin
> ext2spice cthresh 0 rthresh 0

# Ardından ext formatını spice formata dönüştürün
> ext2spice
``` 
Bu adımlardan sonra bulunduğunuz konum içinde `sky130_inv.spice` isminde bir spice dosyası oluşacak.

Bu dosya içinde bazı değişiklikler yapmak gerek. Öncelikle `libs` klasörü altındaki `pshort.lib` ve `nshort.lib` dosyalarını include etmemiz gerek. Ardından model definitionları aşağıda gösterildiği gibi değiştirmek gerek. Bu dosya içinde sadece pmos ve nmos ile ilgili tanımlamalar mevcut. Bizim simülasyon yapabilmemiz için input Vdd ve Vss leri bağlamamız lazım. Bunu aşağıda gösterildiği gibi yapabilirsiniz. Son olarak da input yani Va kısmına kod içinde gösterildiği gibi bir pulse generator bağlamak gerek. 

#### Buraya 20.ss i ekle

Aşağıdaki komutu kullanarak ngspice simülasyonunu yapabilirsiniz.
```sh
# Run following for ngspice simulation
> ngspice sky130_inv.spice
``` 
Bu komutun terminal çıktısı aşağıdaki gibi olacaktır.

#### Buraya 21.ss i ekle

Daha sonra ngspice içerisinde input vs output grafiği görmek için aşağıdaki komutu çalıştırabilirsiniz.

```sh
# Too see out vs in graph with respected to the time
> plot y vs time a
``` 
Grafik çıktısı aşağıdaki gibi olacaktır.
#### Buraya 22.ss i ekle

Bu grafik üzerinden bazı parametreleri ölçmek mümkün.

Rise time ölçmek için gücün %20'si ile %80 arasındaki süreyi ölçmemiz gerek. Gücün %20 si 0.66V da denk geliyor.

#### Buraya 23.ss i ekle

Üstteki görselde %20 güçde iken time ölçümü 2.17984e-09s olduğu görülmektedir.

Çıkış gücü %80 iken yani 2.64V deki görüntü aşağıdaki gibidir.

#### Buraya 24.ss i ekle

Görselde görüldüğü gibi 2.64V de time ölçümü 2.23949e-09s dir.

İki ölçüm arasındaki fark rise time değerini vermektedir.

> 2.23949e-09s - 2.17984e-09s = 0.05965e-09s rise time değeridir.


Aynı formul ile fall time değerini ölçebiliriz. Bu sefer output değerinin logic 1 den logic 0 a düştüğü kenardan ölçüm almamız lazım. Aşağıdaki görselde terminal üzerinde falling edge de gücün %80 ve %20 deki time değerleri görülmektedir.

#### Buraya 25.ss i ekle


Terminalde görüldüğü gibi gücün %80 değeri 
> x0 = 4.05054e-09s, y0 = 2.64V

%20 
> x0 = 4.09254e-09s, y0 = 0.6V

> Fall time 4.09254e-09s - 4.05054e-09s =  0.042e-09s dur.

Cell rise delay time yani propagation delay ölçmek için output rise edge de iken gücün %50 sinde yani 1.65V da  input to output delaye bakmak gerek. Terminal ekranında bu iki değeri görebilirsiniz.

#### Buraya 26.ss i ekle


Bu iki değer arasındaki far propagation delayi vermektedir.
> 2.20699e-09s - 2.15e-09s = 0.05699e-09s propagation delaydir.

Aynı yöntemi kullanıp aynı ölçümleri çıkışın fall edge'inde alırsak cell fall delay değerini bulabilir.

#### Buraya 27.ss i ekle

Yukarıdaki terminal çıktısındaki değerleri kullanarak hesaplama yapabiliriz.

> 4.07512e-09s - 4.05e-09s = 0.02512e-09s cell fall delay.

### SkyWater DRC Rules

Link for SkyWater DRC rules : https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html


```sh
# Go to home directory
> cd

# Download SkyWater lab files
> wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

# Extract the files
> tar xfz drc_tests.tgz

# go to inside labs
> cd drc_tests

# Open any of mag file to see layout
> magic -d XR & 
``` 
Aşağıda örnek labları görebilirsiniz.
#### Buraya 28.ss i ekle

magic toolu açtıktan sonra `File>Open` ile istediğiniz layoutu açabilirsiniz. Aşağıda metal3 için magic ekran görüntüsü mevcut. Her tasarım üzerinde ilgili layoutun hangi rule'a denk geldiği yazmaktadır. Her kuralın ilgili açıkalaması üstte eklediğim linkde mevcut.

#### Buraya 29.ss i ekle


#### Fixing Poly DRC

Öncelikle tkcon ekranından `load poly` komutu ile poly.mag layoutunu açın. Aşağıda ilgili layout gösterilmektedir.

#### Buraya 30.ss i ekle

tkcon ekranından seçtiğimix box ın boyutları görülmektedir.

#### Buraya 32.ss i ekle

SkyWater DRC kurallarından poly.9 a baktığımızda kural şu şekildedir;
Poly resistor spacing to poly or spacing (no overlap) to diff/tap 0.480 µm

#### Buraya 31.ss i ekle

Bu hatayı çözmek için sky130A.tech üzerinde bazı değişiklikler yapmamız gerek. Yapılan değişiklikler aşağıdaki görsellerde gösterilmiştir.

#### Buraya 33.ss i ekle
#### Buraya 34.ss i ekle

Ardından aşağıdaki komutları takip ederek yaptığımız değişkliklerin etkisini görebiliriz. Bu komutlar tkcon ekranına giriniz.

```sh
# load sky130.tech file
> tech load sky130A.tech
> drc check
> drc why
``` 
Görüldüğü gibi ilgili kısmın DRC sorunu çözülmüştür.

#### Buraya 35.ss i ekle

#### Fixing nwell.4 DRC

Since no tap presents in nwell it is implemented incorrectly.

#### Buraya 36.ss i ekle

Aşağıdak görüldüğü gibi sky130A.tech içine yeni rule ekliyoruz.

#### Buraya 37.ss i ekle
#### Buraya 38.ss i ekle

Aşağıdaki komutları takip ederek hatanın giderildiğine bakın
```sh
# load sky130.tech file
> tech load sky130A.tech
# drc full check modunu aktifleştir
> drc style drc(full)
# check drc
> drc check
> drc why
``` 
#### Buraya 39.ss i ekle

## Section 4 - Prelayout Timing Analaysis and Importance of Good Clock Tree

#### Verifying Custom Inverter Layout Design

First check whether IO's are located on the intersecton of vertical and horizontal tracks. There is an `tracks.info` file under `/home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd`. In this file width and pitch information of the all metals both in x and y direction. 

This is what tracks.info includes.

#### Buraya 40.ss i ekle

li1 için intersction kontrolünü kolay yapabilmek için magic tool içindeki grid sizelarını li1 için yazan değeler ile değiştirirsek kolayca görebiliriz.

```sh
# First open inverter design
> magic -T sky130A.tech sky130_inv.mag &

# Then change grid size by using tckon screen
> grid 0.46um 0.34um 0.23um 0.17um
```
We can see that input and output intersect with vertical and horizontal tracks.

#### Buraya 41.ss i ekle

Second rule we need to check is that the width of transistor must be odd multiple of x pitch. As we know from tracks.info file the x witch is 0.46um.

#### Buraya 42.ss i ekle

As we can see the above picture the width of transistor is 1.38um so;
> widht = 1.38um = 0.46um * 3 

This proves the second condition.

Same condition is for heigth of standart cell.

#### Buraya 43.ss i ekle

> Heigth of cell 2.72um = 0.34um * 8

Bu durumda kontrol edildiğine göre tasarımımız hazır diyebiliriz. Önceliklde tasarımı tckon ekranından kaydedelim.

```sh
# save design with different name
> save sky130_vsdinv.mag

# Then exit from design
> exit

# Open new design
> magic -T sky130A.tech sky130_vsdinv.mag &

# Write lef file
> lef write
```
Aşağıda yeni tasarımı görebilirsiniz.
#### Buraya 44.ss i ekle


This is new generated lef file.
#### Buraya 45.ss i ekle

Inverter tasarımının picorv32a içine entegre etmek için öncelikle aşağıdaki adımları uygulamamız gerek.

```sh
# Copy lef file under src of picorv32a
> cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
# Copy typical, slow and fast libs under src
> cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```
Gerekli dosyaları kopyaladıktan sonra config.tcl üzerinde aşağıdaki kodları eklememiz lazım. Bu dosya `/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a` pathi altındadır.

```sh
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```
config.tcl dosyasının görüntüsü aşağıdadır.

#### Buraya 46.ss i ekle

Custom inverter tasarımını OpenLane flowuna eklemek için aşağıdaki adımları takip edin.

```sh
# Go to this directory
> cd Desktop/work/tools/openlane_working_dir/openlane

# open docker
> docker

# Enter OpenLane flow with interactive mode
> ./flow.tcl -interactive

# Enter require package
> package require openlane 0.9

# Initialize design
> prep -design picorv32a

# Use this commands for include new added lef file
> set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
> add_lefs -src $lefs

# run synthesis
> run_synthesis
```

This is the synthesis result of design.

#### Buraya 47.ss i ekle

Görselde göründüğü üzere timing violationlar var. Bunları azaltmak için aşağıdaki stratejileri kullanacağız.

```sh
# prep design again and overwrite you last work
> prep -design picorv32a 19-05_20-01 -overwrite

# Use this commands for include new added lef file
> set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
> add_lefs -src $lefs

# synth strateji more tends to improve time. if it is 3, it will be more tend to reduce area. 
> set ::env(SYNTH_STRATEGY) "DELAY 3"
> set ::env(SYNTH_SIZING) 1
# then run synthesis again
> run_synthesis
```

Now as we see WNS and TNS is 0s.

#### Buraya 48.ss i ekle

Then run floorplan.

```sh
# Now we can run floorplan
> run_floorplan
```
After run the command an error is occured.
#### Buraya 49.ss i ekle

Bunu çözmek için aşağıdaki adımları uygulayın.

```sh
> init_floorplan
> place_io
> tap_decap_or
```
Then run placement
```sh
> run_placement
```
Aşağıda placement sonucu terminal çıktısı mevcut.

#### Buraya 50.ss i ekle

Placement sonucunu görmek için aşağıdaki adımları takip edin.

```sh 
# Go to placement results directory
> cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/19-05_21-10/results/placement

# Open layout with magic tool
> magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Placement sonucu aşağıda gösterilmektedir. Resimde görüldüğü gibi custom inverter bloğumuz place edilmiş.

#### Buraya 51.ss i ekle

tckon ekranına alttaki komutu eklerseniz metal layer layout görülmektedir.
```sh
# metal layer view
> expand
```
#### Buraya 52.ss i ekle


#### Post Synthesis Timing
Bu aşamada base.sdc üzerinde değişiklik yaparak WNS ve TNS de iyileştirmeler yapmaya çalışacağız. Bir önceki çalışmamızda 0s WNS olduğu için stratejilerimizi eski haline alıp negative slack alalaım tekrardan.

```sh
# Go to this directory
> cd Desktop/work/tools/openlane_working_dir/openlane

# open docker
> docker

# Enter OpenLane flow with interactive mode
> ./flow.tcl -interactive

# Enter require package
> package require openlane 0.9

# Initialize design
> prep -design picorv32a

# Use this commands for include new added lef file
> set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
> add_lefs -src $lefs

# run synthesis
> run_synthesis
```
Terminalde göründüğü gibi yine negative slack hatamız var.

#### Buraya 53.ss i ekle

Öncelikle yüksek fanout limitlemesi yapıp tekrar sentez yapıyoruz.

```sh
# Go to this directory
> cd Desktop/work/tools/openlane_working_dir/openlane

# open docker
> docker

# Enter OpenLane flow with interactive mode
> ./flow.tcl -interactive

# Enter require package
> package require openlane 0.9

# Initialize design
> prep -design picorv32a

# Use this commands for include new added lef file
> set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
> add_lefs -src $lefs
> set ::env(SYNTH_SIZING) 1

# Limit fanout
> set ::env(SYNTH_MAX_FANOUT) 4

# run synthesis
> run_synthesis
```
#### Buraya 54.ss i ekle

Yukarıda görüldüğü gibi hala timing sorunu devam etmekte.

Başka terminalden bu komutu çalıştırıp timing detaylarına bakalım.

```sh
> cd Desktop/work/tools/openlane_working_dir/openlane

> sta pre_sta.conf
```
#### Buraya 55.ss i ekle

Burada 4 fanoutu olan bir or gate var. Bunun drive strength değerini 2 den 4 e çıkarıp tekrar deneyelim.

```sh
> report_net -connections _10566_
> replace_cell _13165_ sky130_fd_sc_hd__or3_4
> report_checks -fields {net cap slew input_pins} -digits 4
```
Bu düzeltme ile slack üzerindeki düşüş görülebilir.

#### Buraya 56.ss i ekle

Aynı düzeltmeyi başka bir gate üzerinde yapacağız.

#### Buraya 57.ss i ekle

```sh
> report_net -connections _10534_
> replace_cell _13132_ sky130_fd_sc_hd__or4_4
> report_checks -fields {net cap slew input_pins} -digits 4
```
Terminalde göründüğü gibi slack değeri tekrardan düştü.

#### Buraya 58.ss i ekle

Şimdi bu güncel netlisti flowumuzun içine dahil edeceğiz. Bir yedeğini alıp write_verilog komutu ile overwrite etmemiz lazım.

```sh
# Go to synthesis result directory
> cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/19-05_23-26/results/synthesis

# Yedek al 
> cp picorv32a.synthesis.v picorv32a.synthesis_old.v
```

Ardından sta ekranında aşağıdaki komutu çalıştır.
```sh
# Overwriting current synthesis netlist
> write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/19-05_23-26/results/synthesis/picorv32a.synthesis.v
```
#### Buraya 62.ss i ekle

Ardından CTS adımına gitmek için aşağıdaki komutları kullanın.Bu komutları OpenLane terminalinde çalıştırın.
```sh
# synthesis
> run_synthesis
```

#### Buraya 59.ss i ekle

```sh
# Commands for floorplan
> init_floorplan
> place_io
> tap_decap_or

# placment step
> run_placement
```

#### Buraya 60.ss i ekle

```sh
# Then run cts
> run_cts
```

#### Buraya 61.ss i ekle

Post-CTS adımları için OpenRoad toolunu kullancağız.OpenLane terminali içinde OpenRoad'ı çalıştıralım.

```sh
# Invoke OpenRoad
> openroad

# Reading lef file
> read_lef /openLANE_flow/designs/picorv32a/runs/19-05_23-26/tmp/merged.lef

# Reading def file
> read_def /openLANE_flow/designs/picorv32a/runs/19-05_23-26/results/cts/picorv32a.cts.def

# Create OpenRoad Database
> write_db pico_cts.db

# Read created database
> read_db pico_cts.db

# Read netlist post CTS
> read_verilog /openLANE_flow/designs/picorv32a/runs/19-05_23-26/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
> read_liberty $::env(LIB_SYNTH_COMPLETE)

# Read in the custom sdc we created
> read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
> set_propagated_clock [all_clocks]

# Generate timing report
> report_checks -path_delay min_max -format full_clock_expanded -digits 4
```
#### Buraya 63.ss i ekle
Üstteki raporda göründüğü gibi Slack değerimiz 14.1462ns olmuş durumda.

Bu adımda `'sky130_fd_sc_hd__clkbuf_1` cell i clock buffer listten kaldırıp post-CTS sonuçlarına bakacağız.
```sh
# Remove 'sky130_fd_sc_hd__clkbuf_1' from the list
> set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0

# Setting def as placement def
> set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/19-05_23-26/results/placement/picorv32a.placement.def

# Then run_cts
> run_cts

# Run OpenROAD tool
> openroad

# Reading lef file
> read_lef /openLANE_flow/designs/picorv32a/runs/19-05_23-26/tmp/merged.lef

# Reading def file
> read_def /openLANE_flow/designs/picorv32a/runs/19-05_23-26/results/cts/picorv32a.cts.def

# Create database with custom name
> write_db pico_cts1.db

# Loading the created database
> read_db pico_cts1.db

# Read netlist post CTS
> read_verilog /openLANE_flow/designs/picorv32a/runs/19-05_23-26/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
> read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design
> link_design picorv32a

# Read custom sdc
> read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
> set_propagated_clock [all_clocks]

# Generating custom timing report
> report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
> report_clock_skew -hold

# Report setup skew
> report_clock_skew -setup

# Exit to OpenLANE flow
> exit

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
> set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]
```

# Section 5 - Final steps for RTL2GDS using tritonRoute and openSTA 

Execute Power Distribution Network (PDN) and explore the layout.

```sh
# Go to this directory
> cd Desktop/work/tools/openlane_working_dir/openlane

# open docker
> docker

# Enter OpenLane flow with interactive mode
> ./flow.tcl -interactive

# Enter require package
> package require openlane 0.9

# Initialize design
> prep -design picorv32a

# Use this commands for include new added lef file
> set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
> add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
> set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
> set ::env(SYNTH_SIZING) 1

# run synthesis
> run_synthesis

# Commands for floorplan
> init_floorplan
> place_io
> tap_decap_or

# placment step
> run_placement

# Then run_cts
> run_cts

# Finally generate pdn
> gen_pdn 
```

Bu adımlardan sonra terminal çıktısı aşağıdaki gibi olacaktır.
#### Buraya 64.ss i ekle

PDN den sonra oluşan layoutu görmek için alağıdaki komutları takip edin.

```sh
> cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/20-05_20-50/tmp/floorplan

> magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```
Screenshot of PDN
#### Buraya 65.ss i ekle

Then run routing.

```sh
> run_routing
```

Routing sonucunu terminalden görebilirsiniz.

#### Buraya 66.ss i ekle

Routing sonrası oluşan designı görmek için aşağıdaki adımları takip edin.
```sh
> cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/20-05_20-50/results/routing

> magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```
Routing ekran görüntüleri ektedir.
#### Buraya 67.ss i ekle
#### Buraya 68.ss i ekle


Son olarak extracted parasitler ile post-route OpenSta Timing analiz yapacağız. Bunun için aşağıdaki adımları takip edin.

```sh
# openroad
> openroad

# Reading lef file
> read_lef /openLANE_flow/designs/picorv32a/runs/20-05_20-50/tmp/merged.lef

# Reading def file
> read_def /openLANE_flow/designs/picorv32a/runs/20-05_20-50/results/routing/picorv32a.def

# Create database with custom name
> write_db pico_route.db

# Loading the created database
> read_db pico_route.db

# Read netlist
> read_verilog /openLANE_flow/designs/picorv32a/runs/20-05_20-50/results/synthesis/picorv32a.synthesis_preroute.v

# Read library for design
> read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
> link_design picorv32a

# Read custom sdc
> read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
> set_propagated_clock [all_clocks]

> read_spef /openLANE_flow/designs/picorv32a/runs/20-05_20-50/results/routing/picorv32a.spef

> report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
```

Parasitic extractionlardan sonra slack 14.1462ns den 13.9090ns e düşmüştür. Aşağıda terminal çıktısı mevcut.

#### Buraya 69.ss i ekle

