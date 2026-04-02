Lab 3: Machine Learning สำหรับการจำแนกการใช้ที่ดิน

1. ออกแบบ Training Strategy
	
        ในการศึกษานี้ได้เลือกพื้นที่ศึกษาเป็นจังหวัดนครปฐม โดยใช้ข้อมูลขอบเขตจากชุดข้อมูล FAO GAUL level1 เพื่อให้การวิเคราะห์สอดคล้องกับขอบเขตการปกครองจริง
	      1.1 การกำหนดประเภทการใช้ที่ดิน (Classes) ได้กำหนดประเภทการใช้ที่ดินจำนวน 4 ประเภท 
                •	Agriculture (40) – พื้นที่เกษตรกรรม
                •	Urban (50) – พื้นที่เมือง
                •	Bare land (60) – พื้นที่โล่ง 
                •	Water (80) – พื้นที่แหล่งน้ำ
	          โดยอ้างอิงจากชุดข้อมูล ESA WorldCover 2021 เพื่อใช้เป็นข้อมูลอ้างอิง (reference dataset) สำหรับการสร้าง training samples
	      1.2 การสร้าง Training Samples
		 	  สร้าง training data
   			  	> ใช้วิธี อ้างอิงจาก WorldCover dataset โดยเลือกเฉพาะ class (40, 50, 60, 80) และทำการ mask class อื่นออก เพื่อให้ได้ข้อมูลตัวอย่างที่มีความสอดคล้องกับพื้นที่จริง
   			  การเตรียมข้อมูลภาพดาวเทียม
		 	  	> ใช้ข้อมูลจาก Sentinel-2 Surface Reflectance (S2_SR) โดยกำหนดช่วงเวลาเป็นเดือนมกราคม ถึง มิถุนายน ปี 2021 ซึ่งเป็นช่วงที่มีความเหมาะสมต่อการจำแนกพื้นที่เกษตรกรรมในภาคกลางของประเทศไทย
                มีการใช้ฟังก์ชัน mask
                 	> cloud shadow
                 	> cloud medium / high
                 	> cirrus
                เพื่อตัดค่าที่ไม่ต้องการออกและเพื่อเพิ่มคุณภาพของข้อมูลภาพ
   		1.3 Training and Validation
				> ใช้วิธี Stratified Sampling โดยอ้างอิงจากค่า class ใน WorldCover เพื่อให้แต่ละประเภทการใช้ที่ดินมีจำนวนตัวอย่างที่สมดุล
				> กำหนดจำนวนตัวอย่างทั้งหมด 400 จุดต่อ class และทำการสุ่มข้อมูลด้วย randomColumn เพื่อแบ่งข้อมูลเป็น
					>> Training set (80%)
   					>> Validation set (20%)
		1.4 Feature Selection
				> ใช้ทั้ง Spectral bands และ Spectral indices
					Spectral bands
   						> B2 (Blue)
   						> B3 (Green)
   						> B4 (Red) 
						> B8 (NIR) 
						> B11 (SWIR)
				เพื่อให้ครอบคลุมช่วงคลื่นสำคัญในการแยกประเภทพื้นผิว เช่น น้ำ พืช และสิ่งปลูกสร้าง
					Spectral indices
   						> NDVI (Normalized Difference Vegetation Index)
   						> NDWI (Normalized Difference Water Index)
   						> NDBI (Normalized Difference Built-up Index)
    			เหตุผลในการเลือก Feature คือ การรวม spectral bands กับ indices ช่วยเพิ่มความสามารถของโมเดลในการแยกประเภทพื้นที่ที่มีลักษณะคล้ายกัน

















































