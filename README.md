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
