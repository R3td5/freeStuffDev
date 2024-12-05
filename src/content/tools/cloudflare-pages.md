    <div class="menu">
        <button class="menu-button" onclick="showForm('search')">البحث</button>
        <button class="menu-button" onclick="showForm('display')">عرض العقارات</button>
        <button class="menu-button" onclick="showForm('request')">طلب عقار</button>
    </div>

    <!-- نموذج البحث -->
    <div id="searchForm" class="form-container hidden">
        <h2>البحث عن عقار</h2>
        <div class="search-filters">
            <div class="form-group">
                <label>المحافظة:</label>
                <select id="searchGovernorate" onchange="loadDirectorates(this.value, 'searchDirectorate')">
                    <option value="">اختر المحافظة</option>
                    <option value="صنعاء">صنعاء</option>
                    <option value="عدن">عدن</option>
                    <option value="تعز">تعز</option>
                </select>
            </div>
            <div class="form-group">
                <label>المديرية:</label>
                <select id="searchDirectorate" onchange="loadAreas(this.value, 'searchArea')">
                    <option value="">اختر المديرية</option>
                </select>
            </div>
            <div class="form-group">
                <label>المنطقة:</label>
                <select id="searchArea" onchange="loadDistricts(this.value, 'searchDistrict')">
                    <option value="">اختر المنطقة</option>
                </select>
            </div>
            <div class="form-group">
                <label>الحي:</label>
                <select id="searchDistrict">
                    <option value="">اختر الحي</option>
                </select>
            </div>
            <div class="form-group">
                <label>نوع العقار:</label>
                <select id="searchPropertyType">
                    <option value="">اختر نوع العقار</option>
                    <option value="land">أرض</option>
                    <option value="house">بيت</option>
                    <option value="shop">محل</option>
                </select>
            </div>
        </div>
        <button class="submit-btn" onclick="searchProperties()">بحث</button>
        <div id="searchResults" class="properties-grid"></div>
    </div>

    <!-- نموذج الطلب -->
    <div id="requestForm" class="form-container hidden">
        <h2>طلب عقار</h2>
        <div class="form-group">
            <label>المحافظة:</label>
            <select id="governorate" onchange="loadDirectorates(this.value, 'directorate')">
                <option value="">اختر المحافظة</option>
                <option value="صنعاء">صنعاء</option>
                <option value="عدن">عدن</option>
                <option value="تعز">تعز</option>
            </select>
        </div>
        <div class="form-group">
            <label>المديرية:</label>
            <select id="directorate" onchange="loadAreas(this.value, 'area')">
                <option value="">اختر المديرية</option>
            </select>
        </div>
        <div class="form-group">
            <label>المنطقة:</label>
            <select id="area" onchange="loadDistricts(this.value, 'district')">
                <option value="">اختر المنطقة</option>
            </select>
        </div>
        <div class="form-group">
            <label>الحي:</label>
            <select id="district">
                <option value="">اختر الحي</option>
            </select>
        </div>
        <div class="form-group">
            <label>نوع العقار:</label>
            <select id="propertyType" onchange="showPropertyFields()">
                <option value="">اختر نوع العقار</option>
                <option value="land">أرض</option>
                <option value="house">بيت</option>
                <option value="shop">محل</option>
            </select>
        </div>

        <!-- حقول الأرض -->
        <div id="landFields" class="property-fields hidden">
            <input type="file" accept="image/*" id="landImage">
            <input type="text" placeholder="اسم الأرض" id="landName">
            <textarea placeholder="وصف الأرض" id="landDescription"></textarea>
            <input type="text" placeholder="رقم القطعة" id="plotNumber">
            <input type="number" placeholder="عدد الشوارع المطلة على القطعة" id="streetCount">
            <input type="number" placeholder="سعر القطعة" id="landPrice">
            <input type="date" id="landDate">
            <input type="text" placeholder="مقدم الطلب" id="landRequester">
            <input type="tel" placeholder="رقم الهاتف" id="landPhone">
            <input type="number" placeholder="مساحة العقار" id="landArea">
        </div>

        <!-- حقول البيت -->
        <div id="houseFields" class="property-fields hidden">
            <input type="file" accept="image/*" id="houseImage">
            <textarea placeholder="وصف البيت" id="houseDescription"></textarea>
            <input type="number" placeholder="سعر البيت" id="housePrice">
            <input type="number" placeholder="رقم الطابق" id="floorNumber">
            <input type="date" id="houseDate">
            <input type="text" placeholder="مقدم العرض" id="houseRequester">
            <input type="tel" placeholder="رقم الهاتف" id="housePhone">
            <input type="number" placeholder="مساحة البيت" id="houseArea">
        </div>

        <!-- حقول المحل -->
        <div id="shopFields" class="property-fields hidden">
            <input type="file" accept="image/*" id="shopImage">
            <textarea placeholder="وصف المحل" id="shopDescription"></textarea>
            <input type="number" placeholder="سعر المحل" id="shopPrice">
            <input type="date" id="shopDate">
            <input type="text" placeholder="مقدم العرض" id="shopRequester">
            <input type="tel" placeholder="رقم الهاتف" id="shopPhone">
            <input type="number" placeholder="مساحة المحل" id="shopArea">
        </div>

        <button class="submit-btn" onclick="submitRequest()">تقديم الطلب</button>
    </div>

    <!-- عرض العقارات -->
    <div id="displayProperties" class="form-container hidden">
        <h2>العقارات المعروضة</h2>
        <div id="propertiesList" class="properties-grid"></div>
    </div>
</div>

<script>
    let properties = [];

    function showForm(formType) {
        document.getElementById('searchForm').classList.add('hidden');
        document.getElementById('requestForm').classList.add('hidden');
        document.getElementById('displayProperties').classList.add('hidden');
        document.getElementById(formType + 'Form').classList.remove('hidden');

        if(formType === 'display') {
            displayAllProperties();
        }
    }

    function showPropertyFields() {
        const propertyType = document.getElementById('propertyType').value;
        document.getElementById('landFields').classList.add('hidden');
        document.getElementById('houseFields').classList.add('hidden');
        document.getElementById('shopFields').classList.add('hidden');
        
        if(propertyType) {
            document.getElementById(propertyType + 'Fields').classList.remove('hidden');
        }
    }

    function loadDirectorates(governorate, targetId) {
        const directorates = {
            'صنعاء': ['السبعين', 'معين', 'الثورة'],
            'عدن': ['المنصورة', 'الشيخ عثمان', 'التواهي'],
            'تعز': ['المظفر', 'القاهرة', 'صالة']
        };
        
        const select = document.getElementById(targetId);
        select.innerHTML = '<option value="">اختر المديرية</option>';
        
        if(governorate && directorates[governorate]) {
            directorates[governorate].forEach(dir => {
                select.add(new Option(dir, dir));
            });
        }
    }

    function loadAreas(directorate, targetId) {
        // يمكن إضافة قائمة المناطق حسب المديرية
        const select = document.getElementById(targetId);
        select.innerHTML = '<option value="">اختر المنطقة</option>';
        // أضف المناطق حسب المديرية
    }

    function loadDistricts(area, targetId) {
        // يمكن إضافة قائمة الأحياء حسب المنطقة
        const select = document.getElementById(targetId);
        select.innerHTML = '<option value="">اختر الحي</option>';
        // أضف الأحياء حسب المنطقة
    }

    function submitRequest() {
        const propertyType = document.getElementById('propertyType').value;
        const property = {
            type: propertyType,
            governorate: document.getElementById('governorate').value,
            directorate: document.getElementById('directorate').value,
            area: document.getElementById('area').value,
            district: document.getElementById('district').value,
            date: new Date().toISOString()
        };

        // إضافة الحقول حسب نوع العقار
        if(propertyType === 'land') {
            property.name = document.getElementById('landName').value;
            property.description = document.getElementById('landDescription').value;
            property.plotNumber = document.getElementById('plotNumber').value;
            property.streetCount = document.getElementById('streetCount').value;
            property.price = document.getElementById('landPrice').value;
            property.requester = document.getElementById('landRequester').value;
            property.phone = document.getElementById('landPhone').value;
            property.area = document.getElementById('landArea').value;
        } else if(propertyType === 'house') {
            property.description = document.getElementById('houseDescription').value;
            property.price = document.getElementById('housePrice').value;
            property.floorNumber = document.getElementById('floorNumber').value;
            property.requester = document.getElementById('houseRequester').value;
            property.phone = document.getElementById('housePhone').value;
            property.area = document.getElementById('houseArea').value;
        } else if(propertyType === 'shop') {
            property.description = document.getElementById('shopDescription').value;
            property.price = document.getElementById('shopPrice').value;
            property.requester = document.getElementById('shopRequester').value;
            property.phone = document.getElementById('shopPhone').value;
            property.area = document.getElementById('shopArea').value;
        }

        properties.push(property);
        alert('تم تقديم الطلب بنجاح');
        clearForm();
    }

    function clearForm() {
        document.getElementById('requestForm').reset();
        showPropertyFields();
    }

    function searchProperties() {
        const governorate = document.getElementById('searchGovernorate').value;
        const directorate = document.getElementById('searchDirectorate').value;
        const area = document.getElementById('searchArea').value;
        const district = document.getElementById('searchDistrict').value;
        const propertyType = document.getElementById('searchPropertyType').value;

        const filteredProperties = properties.filter(property => {
            return (!governorate || property.governorate === governorate) &&
                   (!directorate || property.directorate === directorate) &&
                   (!area || property.area === area) &&
                   (!district || property.district === district) &&
                   (!propertyType || property.type === propertyType);
        });

        displayProperties(filteredProperties, 'searchResults');
    }

    function displayAllProperties() {
        displayProperties(properties, 'propertiesList');
    }

    function displayProperties(propertiesToShow, containerId) {
        const container = document.getElementById(containerId);
        container.innerHTML = '';

        if(propertiesToShow.length === 0) {
            container.innerHTML = '<p>لا توجد عقارات متاحة</p>';
            return;
        }

        propertiesToShow.forEach(property => {
            const card = document.createElement('div');
            card.className = 'property-card';
            let propertyTypeArabic = {
                'land': 'أرض',
                'house': 'بيت',
                'shop': 'محل'
            }[property.type];

            card.innerHTML = `
                <h3>${propertyTypeArabic}</h3>
                <p>المحافظة: ${property.governorate}</p>
                <p>المديرية: ${property.directorate}</p>
                <p>المنطقة: ${property.area}</p>
                <p>الحي: ${property.district}</p>
                ${property.description ? `<p>الوصف: ${property.description}</p>` : ''}
                ${property.price ? `<p>السعر: ${property.price}</p>` : ''}
                ${property.area ? `<p>المساحة: ${property.area}</p>` : ''}
                <p>رقم الهاتف: ${property.phone}</p>
            `;
            container.appendChild(card);
        });
    }
</script>
