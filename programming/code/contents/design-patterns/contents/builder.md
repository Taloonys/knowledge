> Паттерн ориентирован на комплексное построение объекта. Проще всего представить это как приготовление бургера, человек хочет бургер, но его наполнение может менять как качественно, так и количественно. Также в основе этого паттерна лежит построение объекта отдельно от его представления.


![Untitled](programming/code/contents/design-patterns/image-storage/Untitled%207.png)

## Субъекты:

### **Director**

- Некоторая сущность, работающая в паре с Builder, которая знает какие этапы лежат в построении продукта/объекта, однако не знает, как именно происходят эти этапы. Может так же указывать, что надо построить в этом продукте.

```cpp
class ComputerAssembler {
public:
	Computer assembleComputer(ComputerBuilder& builder) {
		builder.buildCpu("Intel i7");
		builder.buildMemory("16GB");
		builder.builedStorage("512GB SSD");
		return builder.getResult();
	};
};

```

### **Builder**

- Интерфейс "работяги", описывающий, какие этапы при построения объекта происходят

```cpp
class IComputerBuilder {
public:
	virtual ~IComputerBuildre() noexcept = default;
public:
	virtual void buildCpu(const std::string& cpu) = 0;
	virtual void buildMemory(const std::string& memory) = 0;
	virtual void buildStorage(const std::string& storage) = 0;
	virtual Computer getResult() = 0;
}

```

### **Concrete Builder**

- Класс "работяга", что содержит реализацию построения объекта поэтапно

```cpp
class DesktopComputerBuilder : public IComputerBuilder {
	Computer computer_;
public:
	DesktopComputerBuilder() {
        computer_ = Computer();
    }

    void buildCPU(const std::string& cpu) override {
        computer_.setCPU(cpu);
    }

    void buildMemory(const std::string& memory) override {
        computer_.setMemory(memory);
    }

    void buildStorage(const std::string& storage) override {
        computer_.setStorage(storage);
    }

    Computer getResult() override {
        return computer_;
    }
}

```

### **Product**

- Некоторый сложно конструируемый объект

```cpp
class Computer {
    std::string cpu_;
    std::string memory_;
    std::string storage_;
public:
    void setCPU(const std::string& cpu) {
        cpu_ = cpu;
    }

    void setMemory(const std::string& memory) {
        memory_ = memory;
    }

    void setStorage(const std::string& storage) {
        storage_ = storage;
    }

    void display() {
        std::cout << "CPU: " << cpu_ << std::endl;
        std::cout << "Memory: " << memory_ << std::endl;
        std::cout << "Storage: " << storage_ << std::endl;
    }
};

```

### **Client**

```cpp
int main()
{
	DesktopComputerBuilder desktop_builder;
	ComputerAssembler computer_assembler;

	Computer desktop = assembler.assembleComputer(desktop_builder);

	std::cout << "Desktop Computer Configuration:" << std::endl;
	desktop.display();

	return 0;
}

```

## Нюансы

- Из-за того, что создание объекта частично декларируется из-вне (как бы создаёт по шагам Director, но что его просят создать - запрашивает Client), то есть вероятность, что объект может быть создан не "валидным"