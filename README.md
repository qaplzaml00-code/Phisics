# Phisics
Презентация на тему физика
// Основные переменные для управления презентацией
let currentSlide = 1;
const totalSlides = 22;
let slides = [];

// Инициализация при загрузке страницы
document.addEventListener('DOMContentLoaded', function() {
    // Собираем все слайды
    slides = document.querySelectorAll('.slide');
    
    // Инициализируем меню
    initSlideMenu();
    
    // Инициализируем конвертер
    initConverter();
    
    // Инициализируем калькулятор скорости
    initSpeedCalculator();
    
    // Показываем первый слайд
    showSlide(currentSlide);
    
    // Добавляем обработчики клавиатуры
    document.addEventListener('keydown', handleKeyPress);
    
    // Обновляем отображение номера слайда
    updateSlideCounter();
});

// Функция показа конкретного слайда
function showSlide(slideNumber) {
    // Проверяем границы
    if (slideNumber < 1) slideNumber = 1;
    if (slideNumber > totalSlides) slideNumber = totalSlides;
    
    // Скрываем текущий слайд
    slides.forEach(slide => {
        slide.classList.remove('active');
    });
    
    // Показываем выбранный слайд
    const targetSlide = document.getElementById(`slide${slideNumber}`);
    if (targetSlide) {
        targetSlide.classList.add('active');
        currentSlide = slideNumber;
        updateSlideCounter();
        updateMenuHighlight();
    }
}

// Переход к следующему слайду
function nextSlide() {
    showSlide(currentSlide + 1);
}

// Переход к предыдущему слайду
function prevSlide() {
    showSlide(currentSlide - 1);
}

// Переход к конкретному слайду
function goToSlide(slideNumber) {
    showSlide(slideNumber);
}

// Обновление счетчика слайдов
function updateSlideCounter() {
    document.getElementById('currentSlide').textContent = currentSlide;
    document.getElementById('totalSlides').textContent = totalSlides;
}

// Обработка нажатий клавиш
function handleKeyPress(e) {
    switch(e.key) {
        case 'ArrowRight':
        case 'PageDown':
        case ' ':
            nextSlide();
            e.preventDefault();
            break;
        case 'ArrowLeft':
        case 'PageUp':
            prevSlide();
            e.preventDefault();
            break;
        case 'Home':
            goToSlide(1);
            e.preventDefault();
            break;
        case 'End':
            goToSlide(totalSlides);
            e.preventDefault();
            break;
        case 'Escape':
            toggleMenu();
            break;
    }
}

// Инициализация меню слайдов
function initSlideMenu() {
    const menuToggle = document.getElementById('menuToggle');
    const menuContent = document.getElementById('menuContent');
    const slideMenu = document.getElementById('slideMenu');
    
    // Создаем пункты меню для каждого слайда
    slides.forEach((slide, index) => {
        const slideNumber = index + 1;
        const slideTitle = slide.querySelector('h2') ? slide.querySelector('h2').textContent : `Слайд ${slideNumber}`;
        
        const li = document.createElement('li');
        const a = document.createElement('a');
        a.href = '#';
        a.textContent = `${slideNumber}. ${slideTitle}`;
        a.addEventListener('click', function(e) {
            e.preventDefault();
            goToSlide(slideNumber);
            menuContent.classList.remove('active');
        });
        
        li.appendChild(a);
        slideMenu.appendChild(li);
    });
    
    // Обработчик кнопки меню
    menuToggle.addEventListener('click', function() {
        menuContent.classList.toggle('active');
    });
    
    // Закрытие меню при клике вне его
    document.addEventListener('click', function(e) {
        if (!menuToggle.contains(e.target) && !menuContent.contains(e.target)) {
            menuContent.classList.remove('active');
        }
    });
}

// Обновление выделения в меню
function updateMenuHighlight() {
    const menuLinks = document.querySelectorAll('#slideMenu a');
    menuLinks.forEach(link => {
        link.classList.remove('active');
        const linkSlideNumber = parseInt(link.textContent);
        if (linkSlideNumber === currentSlide) {
            link.classList.add('active');
        }
    });
}

// Переключение видимости меню
function toggleMenu() {
    const menuContent = document.getElementById('menuContent');
    menuContent.classList.toggle('active');
}

// ========== КОНВЕРТЕР ВЕЛИЧИН ==========
function initConverter() {
    const converterType = document.getElementById('converterType');
    const inputUnit = document.getElementById('inputUnit');
    const outputUnit = document.getElementById('outputUnit');
    const inputValue = document.getElementById('inputValue');
    const outputValue = document.getElementById('outputValue');
    const convertButton = document.getElementById('convertButton');
    const swapButton = document.getElementById('swapButton');
    const conversionFormulas = document.getElementById('conversionFormulas');
    const converterExamples = document.getElementById('converterExamples');
    
    // Определения единиц измерения для каждого типа
    const unitDefinitions = {
        temperature: {
            name: "Температура",
            units: [
                { id: 'celsius', name: 'Градусы Цельсия', symbol: '°C' },
                { id: 'fahrenheit', name: 'Градусы Фаренгейта', symbol: '°F' },
                { id: 'kelvin', name: 'Кельвины', symbol: 'K' },
                { id: 'reaumur', name: 'Градусы Реомюра', symbol: '°R' },
                { id: 'rankine', name: 'Градусы Ранкина', symbol: '°Ra' }
            ],
            formulas: {
                'celsius-fahrenheit': '°F = °C × 1.8 + 32',
                'fahrenheit-celsius': '°C = (°F - 32) / 1.8',
                'celsius-kelvin': 'K = °C + 273.15',
                'kelvin-celsius': '°C = K - 273.15',
                'fahrenheit-kelvin': 'K = (°F + 459.67) × 5/9',
                'kelvin-fahrenheit': '°F = K × 9/5 - 459.67'
            }
        },
        length: {
            name: "Длина",
            units: [
                { id: 'meter', name: 'Метры', symbol: 'м' },
                { id: 'kilometer', name: 'Километры', symbol: 'км' },
                { id: 'centimeter', name: 'Сантиметры', symbol: 'см' },
                { id: 'millimeter', name: 'Миллиметры', symbol: 'мм' },
                { id: 'mile', name: 'Мили', symbol: 'миль' },
                { id: 'foot', name: 'Футы', symbol: 'фт' },
                { id: 'inch', name: 'Дюймы', symbol: 'дюйм' },
                { id: 'nautical-mile', name: 'Морские мили', symbol: 'M' }
            ],
            formulas: {
                'meter-kilometer': '1 км = 1000 м',
                'kilometer-meter': '1 м = 0.001 км',
                'meter-centimeter': '1 м = 100 см',
                'centimeter-meter': '1 см = 0.01 м',
                'meter-foot': '1 фт = 0.3048 м',
                'foot-meter': '1 м ≈ 3.2808 фт',
                'kilometer-mile': '1 миля ≈ 1.6093 км',
                'mile-kilometer': '1 км ≈ 0.6214 миль'
            }
        },
        mass: {
            name: "Масса",
            units: [
                { id: 'kilogram', name: 'Килограммы', symbol: 'кг' },
                { id: 'gram', name: 'Граммы', symbol: 'г' },
                { id: 'milligram', name: 'Миллиграммы', symbol: 'мг' },
                { id: 'ton', name: 'Тонны', symbol: 'т' },
                { id: 'pound', name: 'Фунты', symbol: 'lb' },
                { id: 'ounce', name: 'Унции', symbol: 'oz' },
                { id: 'carat', name: 'Караты', symbol: 'ct' }
            ],
            formulas: {
                'kilogram-gram': '1 кг = 1000 г',
                'gram-kilogram': '1 г = 0.001 кг',
                'kilogram-pound': '1 lb ≈ 0.4536 кг',
                'pound-kilogram': '1 кг ≈ 2.2046 lb',
                'kilogram-ounce': '1 oz ≈ 0.02835 кг',
                'ounce-kilogram': '1 кг ≈ 35.274 oz',
                'gram-carat': '1 ct = 0.2 г',
                'carat-gram': '1 г = 5 ct'
            }
        },
        speed: {
            name: "Скорость",
            units: [
                { id: 'mps', name: 'Метры в секунду', symbol: 'м/с' },
                { id: 'kmph', name: 'Километры в час', symbol: 'км/ч' },
                { id: 'mph', name: 'Мили в час', symbol: 'миль/ч' },
                { id: 'knot', name: 'Узлы', symbol: 'уз' },
                { id: 'fps', name: 'Футы в секунду', symbol: 'фт/с' },
                { id: 'mach', name: 'Мах', symbol: 'M' }
            ],
            formulas: {
                'mps-kmph': '1 м/с = 3.6 км/ч',
                'kmph-mps': '1 км/ч ≈ 0.2778 м/с',
                'kmph-mph': '1 км/ч ≈ 0.6214 миль/ч',
                'mph-kmph': '1 миль/ч ≈ 1.6093 км/ч',
                'kmph-knot': '1 км/ч ≈ 0.54 уз',
                'knot-kmph': '1 уз = 1.852 км/ч',
                'mps-mach': '1 M ≈ 343 м/с (в воздухе)',
                'mach-mps': '1 м/с ≈ 0.002915 M'
            }
        }
    };
    
    // Примеры для конвертера
    const examples = {
        temperature: [
            { from: 0, fromUnit: 'celsius', toUnit: 'fahrenheit', result: 32 },
            { from: 100, fromUnit: 'celsius', toUnit: 'fahrenheit', result: 212 },
            { from: -40, fromUnit: 'celsius', toUnit: 'fahrenheit', result: -40 },
            { from: 0, fromUnit: 'celsius', toUnit: 'kelvin', result: 273.15 }
        ],
        length: [
            { from: 1, fromUnit: 'meter', toUnit: 'centimeter', result: 100 },
            { from: 1, fromUnit: 'kilometer', toUnit: 'meter', result: 1000 },
            { from: 1, fromUnit: 'mile', toUnit: 'kilometer', result: 1.6093 },
            { from: 1, fromUnit: 'foot', toUnit: 'meter', result: 0.3048 }
        ],
        mass: [
            { from: 1, fromUnit: 'kilogram', toUnit: 'gram', result: 1000 },
            { from: 1, fromUnit: 'pound', toUnit: 'kilogram', result: 0.4536 },
            { from: 1, fromUnit: 'ounce', toUnit: 'gram', result: 28.35 },
            { from: 1, fromUnit: 'carat', toUnit: 'gram', result: 0.2 }
        ],
        speed: [
            { from: 1, fromUnit: 'mps', toUnit: 'kmph', result: 3.6 },
            { from: 100, fromUnit: 'kmph', toUnit: 'mps', result: 27.78 },
            { from: 1, fromUnit: 'knot', toUnit: 'kmph', result: 1.852 },
            { from: 60, fromUnit: 'kmph', toUnit: 'mph', result: 37.28 }
        ]
    };
    
    // Функции конвертации
    const conversionFunctions = {
        // Температура
        'celsius-fahrenheit': (val) => val * 1.8 + 32,
        'fahrenheit-celsius': (val) => (val - 32) / 1.8,
        'celsius-kelvin': (val) => val + 273.15,
        'kelvin-celsius': (val) => val - 273.15,
        'fahrenheit-kelvin': (val) => (val + 459.67) * 5/9,
        'kelvin-fahrenheit': (val) => val * 9/5 - 459.67,
        'celsius-reaumur': (val) => val * 0.8,
        'reaumur-celsius': (val) => val / 0.8,
        'celsius-rankine': (val) => (val + 273.15) * 9/5,
        'rankine-celsius': (val) => (val - 491.67) * 5/9,
        
        // Длина
        'meter-kilometer': (val) => val / 1000,
        'kilometer-meter': (val) => val * 1000,
        'meter-centimeter': (val) => val * 100,
        'centimeter-meter': (val) => val / 100,
        'meter-millimeter': (val) => val * 1000,
        'millimeter-meter': (val) => val / 1000,
        'meter-mile': (val) => val / 1609.344,
        'mile-meter': (val) => val * 1609.344,
        'meter-foot': (val) => val / 0.3048,
        'foot-meter': (val) => val * 0.3048,
        'meter-inch': (val) => val / 0.0254,
        'inch-meter': (val) => val * 0.0254,
        'meter-nautical-mile': (val) => val / 1852,
        'nautical-mile-meter': (val) => val * 1852,
        
        // Масса
        'kilogram-gram': (val) => val * 1000,
        'gram-kilogram': (val) => val / 1000,
        'kilogram-milligram': (val) => val * 1000000,
        'milligram-kilogram': (val) => val / 1000000,
        'kilogram-ton': (val) => val / 1000,
        'ton-kilogram': (val) => val * 1000,
        'kilogram-pound': (val) => val / 0.45359237,
        'pound-kilogram': (val) => val * 0.45359237,
        'kilogram-ounce': (val) => val / 0.028349523125,
        'ounce-kilogram': (val) => val * 0.028349523125,
        'kilogram-carat': (val) => val * 5000,
        'carat-kilogram': (val) => val / 5000,
        
        // Скорость
        'mps-kmph': (val) => val * 3.6,
        'kmph-mps': (val) => val / 3.6,
        'mps-mph': (val) => val * 2.23694,
        'mph-mps': (val) => val / 2.23694,
        'mps-knot': (val) => val / 0.514444,
        'knot-mps': (val) => val * 0.514444,
        'mps-fps': (val) => val / 0.3048,
        'fps-mps': (val) => val * 0.3048,
        'mps-mach': (val) => val / 343,
        'mach-mps': (val) => val * 343,
        'kmph-mph': (val) => val * 0.621371,
        'mph-kmph': (val) => val / 0.621371,
        'kmph-knot': (val) => val * 0.539957,
        'knot-kmph': (val) => val / 0.539957
    };
    
    // Заполнение единицами измерения при изменении типа
    function populateUnits() {
        const type = converterType.value;
        const units = unitDefinitions[type].units;
        
        // Очищаем select
        inputUnit.innerHTML = '';
        outputUnit.innerHTML = '';
        
        // Заполняем options
        units.forEach(unit => {
            const inputOption = document.createElement('option');
            inputOption.value = unit.id;
            inputOption.textContent = `${unit.name} (${unit.symbol})`;
            
            const outputOption = document.createElement('option');
            outputOption.value = unit.id;
            outputOption.textContent = `${unit.name} (${unit.symbol})`;
            
            inputUnit.appendChild(inputOption);
            outputUnit.appendChild(outputOption);
        });
        
        // Устанавливаем значения по умолчанию
        if (type === 'temperature') {
            inputUnit.value = 'celsius';
            outputUnit.value = 'fahrenheit';
        } else if (type === 'length') {
            inputUnit.value = 'meter';
            outputUnit.value = 'kilometer';
        } else if (type === 'mass') {
            inputUnit.value = 'kilogram';
            outputUnit.value = 'gram';
        } else if (type === 'speed') {
            inputUnit.value = 'mps';
            outputUnit.value = 'kmph';
        }
        
        // Обновляем формулы
        updateFormulas();
        
        // Обновляем примеры
        updateExamples();
    }
    
    // Обновление формул преобразования
    function updateFormulas() {
        const type = converterType.value;
        const formulas = unitDefinitions[type].formulas;
        
        conversionFormulas.innerHTML = '';
        
        for (const [key, formula] of Object.entries(formulas)) {
            const formulaDiv = document.createElement('div');
            formulaDiv.className = 'formula-item';
            formulaDiv.innerHTML = `<p><strong>${key.replace('-', ' → ')}:</strong> ${formula}</p>`;
            conversionFormulas.appendChild(formulaDiv);
        }
    }
    
    // Обновление примеров
    function updateExamples() {
        const type = converterType.value;
        const typeExamples = examples[type];
        
        converterExamples.innerHTML = '';
        
        typeExamples.forEach(example => {
            const exampleDiv = document.createElement('div');
            exampleDiv.className = 'example-item';
            
            // Находим символы единиц
            const fromUnitSymbol = unitDefinitions[type].units.find(u => u.id === example.fromUnit)?.symbol || '';
            const toUnitSymbol = unitDefinitions[type].units.find(u => u.id === example.toUnit)?.symbol || '';
            
            exampleDiv.innerHTML = `
                <p><strong>${example.from} ${fromUnitSymbol}</strong> = <strong>${example.result.toFixed(4)} ${toUnitSymbol}</strong></p>
                <small>${example.fromUnit} → ${example.toUnit}</small>
            `;
            
            converterExamples.appendChild(exampleDiv);
        });
    }
    
    // Функция конвертации
    function convert() {
        const inputVal = parseFloat(inputValue.value);
        
        if (isNaN(inputVal)) {
            alert('Пожалуйста, введите числовое значение');
            return;
        }
        
        const fromUnit = inputUnit.value;
        const toUnit = outputUnit.value;
        const type = converterType.value;
        
        // Если единицы совпадают
        if (fromUnit === toUnit) {
            outputValue.value = inputVal;
            return;
        }
        
        // Проверяем, есть ли прямая функция конвертации
        const conversionKey = `${fromUnit}-${toUnit}`;
        
        if (conversionFunctions[conversionKey]) {
            const result = conversionFunctions[conversionKey](inputVal);
            outputValue.value = result.toFixed(6);
        } else {
            // Пробуем через промежуточную конвертацию (например, через базовую единицу)
            // Для температуры базовой является Celsius
            // Для длины - метр
            // Для массы - килограмм
            // Для скорости - м/с
            
            let result = inputVal;
            
            // Конвертируем из исходной единицы в базовую
            const baseUnit = getBaseUnit(type);
            if (fromUnit !== baseUnit) {
                const toBaseKey = `${fromUnit}-${baseUnit}`;
                if (conversionFunctions[toBaseKey]) {
                    result = conversionFunctions[toBaseKey](result);
                }
            }
            
            // Конвертируем из базовой в целевую
            if (toUnit !== baseUnit) {
                const fromBaseKey = `${baseUnit}-${toUnit}`;
                if (conversionFunctions[fromBaseKey]) {
                    result = conversionFunctions[fromBaseKey](result);
                }
            }
            
            outputValue.value = result.toFixed(6);
        }
    }
    
    // Получение базовой единицы для типа
    function getBaseUnit(type) {
        switch(type) {
            case 'temperature': return 'celsius';
            case 'length': return 'meter';
            case 'mass': return 'kilogram';
            case 'speed': return 'mps';
            default: return '';
        }
    }
    
    // Функция обмена единицами местами
    function swapUnits() {
        const tempUnit = inputUnit.value;
        const tempValue = inputValue.value;
        
        inputUnit.value = outputUnit.value;
        outputUnit.value = tempUnit;
        
        // Если есть значение в output, перемещаем его в input
        if (outputValue.value) {
            inputValue.value = outputValue.value;
            outputValue.value = '';
        } else if (tempValue) {
            // Конвертируем обратно
            convert();
        }
    }
    
    // Инициализация при загрузке
    populateUnits();
    
    // Обработчики событий
    converterType.addEventListener('change', populateUnits);
    convertButton.addEventListener('click', convert);
    swapButton.addEventListener('click', swapUnits);
    
    // Конвертация при изменении значения
    inputValue.addEventListener('input', function() {
        if (this.value && !isNaN(parseFloat(this.value))) {
            convert();
        } else {
            outputValue.value = '';
        }
    });
    
    // Конвертация при изменении единиц
    inputUnit.addEventListener('change', convert);
    outputUnit.addEventListener('change', convert);
}

// ========== КАЛЬКУЛЯТОР СКОРОСТИ ==========
function initSpeedCalculator() {
    const speedInput = document.getElementById('speedInput');
    const timeInput = document.getElementById('timeInput');
    const distanceInput = document.getElementById('distanceInput');
    const speedUnit = document.getElementById('speedUnit');
    const timeUnit = document.getElementById('timeUnit');
    const distanceUnit = document.getElementById('distanceUnit');
    const calculateButton = document.getElementById('calculateButton');
    const clearButton = document.getElementById('clearButton');
    const calculationResult = document.getElementById('calculationResult');
    const currentFormula = document.getElementById('currentFormula');
    
    // Коэффициенты преобразования единиц
    const speedFactors = {
        'm/s': 1,
        'km/h': 1/3.6,
        'mph': 0.44704,
        'knot': 0.514444
    };
    
    const timeFactors = {
        's': 1,
        'min': 60,
        'h': 3600,
        'day': 86400
    };
    
    const distanceFactors = {
        'm': 1,
        'km': 1000,
        'mile': 1609.344,
        'nmi': 1852
    };
    
    // Функция расчета
    function calculate() {
        const speed = parseFloat(speedInput.value);
        const time = parseFloat(timeInput.value);
        const distance = parseFloat(distanceInput.value);
        
        // Подсчитываем, сколько полей заполнено
        const filledFields = [speed, time, distance].filter(val => !isNaN(val)).length;
        
        if (filledFields < 2) {
            calculationResult.innerHTML = '<p class="error">Пожалуйста, заполните два любых поля для расчета третьего.</p>';
            return;
        }
        
        if (filledFields === 3) {
            calculationResult.innerHTML = '<p class="error">Пожалуйста, оставьте одно поле пустым для расчета.</p>';
            return;
        }
        
        // Приводим все к базовым единицам (м, с, м/с)
        let speedInMps = !isNaN(speed) ? speed * speedFactors[speedUnit.value] : NaN;
        let timeInSec = !isNaN(time) ? time * timeFactors[timeUnit.value] : NaN;
        let distanceInM = !isNaN(distance) ? distance * distanceFactors[distanceUnit.value] : NaN;
        
        let result = '';
        
        // Рассчитываем недостающее значение
        if (isNaN(speed)) {
            // Расчет скорости: v = s / t
            speedInMps = distanceInM / timeInSec;
            const convertedSpeed = convertSpeedFromMps(speedInMps, speedUnit.value);
            
            result = `
                <p><strong>Скорость (v):</strong> ${convertedSpeed.toFixed(4)} ${speedUnit.value}</p>
                <p><strong>Использованная формула:</strong> v = s / t</p>
                <p>Расчет: ${distanceInM.toFixed(2)} м / ${timeInSec.toFixed(2)} с = ${speedInMps.toFixed(4)} м/с</p>
                <p>Конвертировано в ${speedUnit.value}: ${convertedSpeed.toFixed(4)}</p>
            `;
            
            speedInput.value = convertedSpeed.toFixed(4);
            currentFormula.textContent = 'v = s / t';
            
        } else if (isNaN(time)) {
            // Расчет времени: t = s / v
            timeInSec = distanceInM / speedInMps;
            const convertedTime = convertTimeFromSec(timeInSec, timeUnit.value);
            
            result = `
                <p><strong>Время (t):</strong> ${convertedTime.toFixed(4)} ${timeUnit.value}</p>
                <p><strong>Использованная формула:</strong> t = s / v</p>
                <p>Расчет: ${distanceInM.toFixed(2)} м / ${speedInMps.toFixed(4)} м/с = ${timeInSec.toFixed(2)} с</p>
                <p>Конвертировано в ${timeUnit.value}: ${convertedTime.toFixed(4)}</p>
            `;
            
            timeInput.value = convertedTime.toFixed(4);
            currentFormula.textContent = 't = s / v';
            
        } else if (isNaN(distance)) {
            // Расчет расстояния: s = v × t
            distanceInM = speedInMps * timeInSec;
            const convertedDistance = convertDistanceFromM(distanceInM, distanceUnit.value);
            
            result = `
                <p><strong>Расстояние (s):</strong> ${convertedDistance.toFixed(4)} ${distanceUnit.value}</p>
                <p><strong>Использованная формула:</strong> s = v × t</p>
                <p>Расчет: ${speedInMps.toFixed(4)} м/с × ${timeInSec.toFixed(2)} с = ${distanceInM.toFixed(2)} м</p>
                <p>Конвертировано в ${distanceUnit.value}: ${convertedDistance.toFixed(4)}</p>
            `;
            
            distanceInput.value = convertedDistance.toFixed(4);
            currentFormula.textContent = 's = v × t';
        }
        
        calculationResult.innerHTML = result;
    }
    
    // Функция конвертации скорости из м/с в нужные единицы
    function convertSpeedFromMps(speedInMps, targetUnit) {
        switch(targetUnit) {
            case 'm/s': return speedInMps;
            case 'km/h': return speedInMps * 3.6;
            case 'mph': return speedInMps / 0.44704;
            case 'knot': return speedInMps / 0.514444;
            default: return speedInMps;
        }
    }
    
    // Функция конвертации времени из секунд в нужные единицы
    function convertTimeFromSec(timeInSec, targetUnit) {
        switch(targetUnit) {
            case 's': return timeInSec;
            case 'min': return timeInSec / 60;
            case 'h': return timeInSec / 3600;
            case 'day': return timeInSec / 86400;
            default: return timeInSec;
        }
    }
    
    // Функция конвертации расстояния из метров в нужные единицы
    function convertDistanceFromM(distanceInM, targetUnit) {
        switch(targetUnit) {
            case 'm': return distanceInM;
            case 'km': return distanceInM / 1000;
            case 'mile': return distanceInM / 1609.344;
            case 'nmi': return distanceInM / 1852;
            default: return distanceInM;
        }
    }
    
    // Функция очистки
    function clearFields() {
        speedInput.value = '';
        timeInput.value = '';
        distanceInput.value = '';
        calculationResult.innerHTML = '<p>Введите два значения для расчета третьего.</p>';
        currentFormula.textContent = 'v = s / t';
    }
    
    // Обработчики событий
    calculateButton.addEventListener('click', calculate);
    clearButton.addEventListener('click', clearFields);
    
    // Расчет при изменении любого поля
    [speedInput, timeInput, distanceInput, speedUnit, timeUnit, distanceUnit].forEach(element => {
        element.addEventListener('input', function() {
            // Ждем немного, чтобы пользователь закончил ввод
            clearTimeout(this.timer);
            this.timer = setTimeout(calculate, 500);
        });
    });
}

// Стили для ошибок
const style = document.createElement('style');
style.textContent = `
    .error {
        color: #d32f2f;
        font-weight: bold;
        padding: 10px;
        background: #ffebee;
        border-radius: 5px;
        border-left: 4px solid #d32f2f;
    }
    
    .example-item {
        background: #f5f5f5;
        padding: 15px;
        border-radius: 8px;
        margin: 10px 0;
        border-left: 4px solid #4caf50;
    }
    
    .example-item p {
        margin: 5px 0;
    }
    
    .example-item small {
        color: #666;
        font-size: 0.9rem;
    }
`;
document.head.appendChild(style);
