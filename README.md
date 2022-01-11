# StudentManagement
Use Linked List

- source code quản lý sinh viên bằng danh sách liên kết đơn.
Đây là code của mình.
<details><summary>Xem code</summary>
<p>

```java
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct
{
    char name[25];
    int age;
    float averageScore;
} Student;

typedef struct node
{
    Student data;
    struct node *next;
} node;

node *first = NULL;

Student inputStudent();
node *createNode();
void addFirstNode();
void addLastNode();
void deleteNode();
void inputNewStudentsList();
void addStudent();
node *findStudenToName();
void findStudent();
int deleteStudentToName();
int modifyStudentToName();
void infoStudentTable();
void infoStudentsList();
void deleteStudentsList();
void menu();


// Nhập sinh viên
Student inputStudent() {
    Student sv;
    printf("Nhập Họ Và Tên: ");
    fflush(stdin);
    gets(sv.name);

    printf("Nhập Tuổi: ");
    scanf("%d", &sv.age);

    printf("Nhập Điểm Trung Bình: ");
    scanf("%f", &sv.averageScore);
    return sv;
}

// Tạo node
node *createNode() {
    //cap phat
    node *pnode = (node *)malloc(sizeof(node));
    //truyen du lieu
    pnode -> data = inputStudent();
    // con tro next
    pnode -> next = NULL;
    return pnode;
}

// Thêm node vào đầu
void addFirstNode() {
    node *pnode = createNode();
    if(first == NULL){
        first = pnode;
    } else {
        pnode -> next = first;
        first = pnode;
    }
}

// Thêm node vào cuối
void addLastNode() {
    node *pnode = createNode();
    if (first == NULL) {
        first = pnode;
    } else {
        node *checkLast;
        for (checkLast = first; checkLast -> next != NULL; checkLast = checkLast -> next);
        checkLast -> next = pnode;
    }
}

// Xóa node
void deleteNode(node *del) {
    if(first == NULL){
        printf("Không có gì để xóa hết!\n");
    } else {
        // xóa đầu    
        if(del == first){
            first = first -> next;
            free(first);
        } else { // xóa vị trí bất kì
            node *i;
            for (i = first; i != NULL; i = i->next) {
                if (i -> next == del) {
                    del = i -> next;
                    i -> next = del -> next;
                    free(del);
                }
            }
        }
    }
    
}

// Nhập danh sách sinh viên
void inputNewStudentsList() {
    deleteStudentsList();

    while (1) {
        addLastNode();
        char chose;
        printf("\tNhập tiếp không? y/n: ");
        fflush(stdin);
        chose = getchar();
        if (chose == 'n' || chose == 'N')
            break;
    }
}

// Thêm sinh viên
void addStudent() {
    addLastNode();
}

// Tìm sinh viên theo tên
node *findStudenToName(char findName[]) {
    if (first == NULL) {
        return NULL;
    }
    for (node *i = first; i != NULL; i = i->next) {
        if (strcmp(i -> data.name, findName) == 0)
            return i;
    }
    return NULL;
}

void findStudent(char findName[]){
    node *i = findStudenToName(findName);
    if (i == NULL){
        printf("Không tìm thấy sinh viên\n");
    } else {
        printf("Đã tìm thấy sinh viên trong danh sách\n");
        printf("|%25s|%8s|%8s|\n", "------------------------", "--------", "--------");
        printf("|%25s|%8s|%8s|\n", "HO VA TEN", "TUOI", "DIEMTB");
        printf("|%25s|%8s|%8s|\n", "------------------------", "--------", "--------");
        infoStudentTable(i->data);
        printf("|%25s|%8s|%8s|\n", "------------------------", "--------", "--------");
    }
}

// Xóa sinh viên theo tên
int deleteStudentToName(char findName[])
{
    node *i = findStudenToName(findName);
    if (i == NULL)
        return printf("Không tìm thấy sinh viên");
    deleteNode(i);
    return 1;
}

// Sửa thông tin sinh viên theo tên
int modifyStudentToName(char findName[])
{
    node *i = findStudenToName(findName);
    if (i == NULL)
        return printf("Không tìm thấy sinh viên");
    printf("Nhập thông tin sinh viên\n");
    i -> data = inputStudent();
    return 1;
}

// Hiển thị từng sinh viên
void infoStudentTable(Student sv) {
    printf("|%25s|%8d|%8.2f|\n", sv.name, sv.age, sv.averageScore);
}

// Hiển thị danh sách sinh viên
void infoStudentsList() {
    if (first == NULL) {
        printf("Danh sách rỗng\n");
        return;
    }
    int STT = 1;
    printf("\t\tDANH SACH SINH VIEN\n");
    printf("\t|%4s|%25s|%8s|%8s|\n", "----", "------------------------", "--------", "--------");
    printf("\t|%4s|%25s|%8s|%8s|\n", "STT", "HO VA TEN", "TUOI", "DIEMTB");
    printf("\t|%4s|%25s|%8s|%8s|\n", "----", "------------------------", "--------", "--------");
    for (node *i = first; i != NULL; i = i->next)
    {
        printf("\t|%4d", STT++);
        infoStudentTable(i->data);
    }
    printf("\t|%4s|%25s|%8s|%8s|\n", "----", "------------------------", "--------", "--------");

}

// Xóa danh sách sinh viên
void deleteStudentsList()
{
    while (first != NULL)
    {
        deleteNode(first);
    }
}

// Menu
void menu() {
    printf("******************************************\n");
    printf("**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **\n");
    printf("**    1. Tạo danh sách sinh viên mới    **\n");
    printf("**    2. Hiển thị danh sách sinh viên   **\n");
    printf("**    3. Thêm sinh viên mới             **\n");
    printf("**    4. Sửa thông tin sinh viên        **\n");
    printf("**    5. Tìm sinh viên trong danh sách  **\n");
    printf("**    6. Xóa sinh viên                  **\n");
    printf("**    7. Xóa danh sách sinh viên        **\n");
    printf("**    8. Kết thúc                       **\n");
    printf("******************************************\n");
    printf("**    Nhập lựa chọn của bạn:            **\n");

}

int main()
{
    char findName[25];
    int chose;

    while (1) {
        menu();
        scanf("%d", &chose);
        system("cls");
        switch(chose) {
        case 1:
            printf("Bạn đã chọn tạo danh sách sinh viên mới\n");
            inputNewStudentsList();
            printf("Bạn đã tạo thành công\n");
            break;
        case 2:
            printf("Bạn đã chọn hiển thị danh sách sinh viên\n");
            infoStudentsList();
            printf("Bạn đã hiển thị thành công\n");
            break;
        case 3:
            printf("Bạn đã chọn thêm sinh viên mới\n");
            addStudent();
            printf("Bạn đã thêm thành công\n");
            break;
        case 4:
            printf("Bạn đã chọn sửa thông tin sinh viên\n");
            printf("Nhập tên sinh viên bạn muốn sửa\n");
            fflush(stdin);
            gets(findName);
            if(modifyStudentToName(findName)){
                printf("Bạn đã sửa thành công\n");
            } else {
                printf("Sửa thất bại\n");
            }
            break;
        case 5: 
            printf("Bạn đã chọn tìm một sinh viên\n");
            printf("Nhập tên sinh viên bạn muốn tìm\n");
            fflush(stdin);
            gets(findName);
            findStudent(findName);
            break;
        case 6:
            printf("Bạn đã chọn xóa một sinh viên\n");
            printf("Nhập tên sinh viên bạn muốn xóa\n");
            fflush(stdin);
            gets(findName);
            if(deleteStudentToName(findName)){
                printf("Bạn đã xóa thành công\n");
            } else {
                printf("Bạn đã xóa thất bại");
            }
            break;
        case 7:
            printf("Bạn đã chọn xóa toàn bộ danh sách sinh viên\n");
            deleteStudentsList();
            printf("Bạn đã xóa thành công\n");
            break;
        case 8:
            printf("Bạn đã kết thúc chương trình\n");
            exit(0);
        default:
            printf("Vui lòng nhập từ 1 -> 8\n");
            break;
        }
        printf("Bấm một phím bất kì để  trở về menu\n");
        fflush(stdin);

        system("cls");
    }

    return 0;
}

```

</p>
</details>

- code gồm các chức năng như sau:
```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
```


- khi chọn chức năng thứ 1:
<details><summary>Xem Demo</summary>
<p>

```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
1
Bạn đã chọn tạo danh sách sinh viên mới
warning: this program uses gets(), which is unsafe.
Nhập Họ Và Tên: Doan van son
Nhập Tuổi: 20
Nhập Điểm Trung Bình: 9.1
        Nhập tiếp không? y/n: n
Bạn đã tạo thành công
Bấm một phím bất kì để  trở về menu
```
</p>
</details>


- khi chọn chức năng thứ 2:
<details><summary>Xem Demo</summary>
<p>

```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
2
Bạn đã chọn hiển thị danh sách sinh viên
                DANH SACH SINH VIEN
        |----| ------------------------|--------|--------|
        | STT|                HO VA TEN|    TUOI|  DIEMTB|
        |----| ------------------------|--------|--------|
        |   1|             Doan van son|      20|    9.10|
        |----| ------------------------|--------|--------|
Bạn đã hiển thị thành công
Bấm một phím bất kì để  trở về menu
```
</p>
</details>


- khi chọn chức năng thứ 3:
<details><summary>Xem Demo</summary>
<p>

```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
3
Bạn đã chọn thêm sinh viên mới
Nhập Họ Và Tên: Nguyen Thi Thuy
Nhập Tuổi: 20
Nhập Điểm Trung Bình: 9.2
Bạn đã thêm thành công
Bấm một phím bất kì để  trở về menu
```

==> Sau khi thêm sinh viên:
```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
2
Bạn đã chọn hiển thị danh sách sinh viên
                DANH SACH SINH VIEN
        |----| ------------------------|--------|--------|
        | STT|                HO VA TEN|    TUOI|  DIEMTB|
        |----| ------------------------|--------|--------|
        |   1|             Doan van son|      20|    9.10|
        |   2|          Nguyen Thi Thuy|      20|    9.20|
        |----| ------------------------|--------|--------|
Bạn đã hiển thị thành công
Bấm một phím bất kì để  trở về menu
```

</p>
</details>

- khi chọn chức năng thứ 4:
<details><summary>Xem Demo</summary>
<p>

```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
4
Bạn đã chọn sửa thông tin sinh viên
Nhập tên sinh viên bạn muốn sửa
Doan van son
Nhập thông tin sinh viên
Nhập Họ Và Tên: Van Son
Nhập Tuổi: 20
Nhập Điểm Trung Bình: 9.1
Bạn đã sửa thành công
Bấm một phím bất kì để  trở về menu
```

==> Sau khi sửa thông tin sinh viên:
```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
2
Bạn đã chọn hiển thị danh sách sinh viên
                DANH SACH SINH VIEN
        |----| ------------------------|--------|--------|
        | STT|                HO VA TEN|    TUOI|  DIEMTB|
        |----| ------------------------|--------|--------|
        |   1|                  Van Son|      20|    9.10|
        |   2|          Nguyen Thi Thuy|      20|    9.20|
        |----| ------------------------|--------|--------|
Bạn đã hiển thị thành công
Bấm một phím bất kì để  trở về menu
```
</p>
</details>


- khi chọn chức năng thứ 5:
<details><summary>Xem Demo</summary>
<p>

```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
5
Bạn đã chọn tìm một sinh viên
Nhập tên sinh viên bạn muốn tìm
Van Son
Đã tìm thấy sinh viên trong danh sách
| ------------------------|--------|--------|
|                HO VA TEN|    TUOI|  DIEMTB|
| ------------------------|--------|--------|
|                  Van Son|      20|    9.10|
| ------------------------|--------|--------|
Bấm một phím bất kì để  trở về menu
```
</p>
</details>


- khi chọn chức năng thứ 6:
<details><summary>Xem Demo</summary>
<p>

```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
6
Bạn đã chọn xóa một sinh viên
Nhập tên sinh viên bạn muốn xóa
Nguyen Thi Thuy
Bạn đã xóa thành công
Bấm một phím bất kì để  trở về menu
```
==> Sau khi xóa một sinh viên:
```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
2
Bạn đã chọn hiển thị danh sách sinh viên
                DANH SACH SINH VIEN
        |----| ------------------------|--------|--------|
        | STT|                HO VA TEN|    TUOI|  DIEMTB|
        |----| ------------------------|--------|--------|
        |   1|                  Van Son|      20|    9.10|
        |----| ------------------------|--------|--------|
Bạn đã hiển thị thành công
Bấm một phím bất kì để  trở về menu
```

</p>
</details>

- khi chọn chức năng thứ 7:
<details><summary>Xem Demo</summary>
<p>

```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
7
Bạn đã chọn xóa toàn bộ danh sách sinh viên
Bạn đã xóa thành công
Bấm một phím bất kì để  trở về menu
```

==> sau khi xóa toàn bộ danh sách sinh viên:
```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
2
Bạn đã chọn hiển thị danh sách sinh viên
Danh sách rỗng
Bạn đã hiển thị thành công
Bấm một phím bất kì để  trở về menu
```

</p>
</details>


- khi chọn chức năng thứ 8:
<details><summary>Xem Demo</summary>
<p>

```
******************************************
**    CHƯƠNG TRÌNH QUẢN LÝ SINH VIÊN    **
**    1. Tạo danh sách sinh viên mới    **
**    2. Hiển thị danh sách sinh viên   **
**    3. Thêm sinh viên mới             **
**    4. Sửa thông tin sinh viên        **
**    5. Tìm sinh viên trong danh sách  **
**    6. Xóa sinh viên                  **
**    7. Xóa danh sách sinh viên        **
**    8. Kết thúc                       **
******************************************
**    Nhập lựa chọn của bạn:            **
8
Bạn đã kết thúc chương trình
```

</p>
</details>


