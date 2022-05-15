# pip build

### Build a custom package
1. Thường sẽ dùng setuptools để build package
	- Cài đặt
		  
	  ``` bash
	  pip install --upgrade setuptools
	  ```
		  
	- Một số files cần thiết
		- <span style="color:blue">Setup.cfg</span>: File này gồm các thông tin static về package
		- <span style="color:blue">Setup.py</span>: File này gồm các thông tin dynamic, tùy theo hoàn cảnh của package. Có thể kết hợp cả Setup.cfg và Setup.py với nhau. <span style="color:blue">Setup.py</span> sẽ được ưu tiên hơn. Tuy nhiên nếu không có yêu cầu gì đặc biệt việc build, nên sử dụng file <span style="color:blue">Setup.cfg</span>.
		- <span style="color:blue">MANIFEST.in</span>: Chứa các thông tin về package data cần include trong pakage
		- <span style="color:blue">Pyproject.toml</span>: Thay thế cho file <span style="color:blue">Setup.py</span>

2. Config
	- Set  name cho package
	  
	  ``` python
	   setup( 
			name="mobifone",
			package_dir={"mobifone": "plugins"},
		)
	  ```
	- Chỉ định thư mục chứa cource code
	- Include none [[../Python]] data.