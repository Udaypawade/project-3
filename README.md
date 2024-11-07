#include <stdio.h>
#include <string.h>
#include <ctype.h>

struct Patient {
    char name[100];
    int age;
    char address[200];
    char mobile[15];
    char admitDate[11];
    int admithour;
    int wardType;
    int checkup;
    int doctorType;
};

int isValidPhoneNumber(char *phone) {
    if (strlen(phone) != 10) {
        return 0;
    }
    for (int i = 0; i < 10; i++) {
        if (!isdigit(phone[i])) {
            return 0;
        }
    }
    return 1;
}

int getDoctorFee(int doctorType) {
    switch (doctorType) {
        case 1: return 300;
        case 2: return 500;
        case 3: return 800;
        default: return 0;
    }
}

int main() {
    struct Patient p;
    int hoursAdmitted;
    int totalBill = 0;

    printf("----- Balaji Hospital Billing System -----\n");
    printf("contact info...9456182375\n");
    printf("website to visit....www.balaji.com\n");

    printf("Enter patient's name: ");
    fgets(p.name, sizeof(p.name), stdin);
    p.name[strcspn(p.name, "\n")] = 0;

    printf("Enter patient's age: ");
    scanf("%d", &p.age);

    printf("Enter patient's address: ");
    scanf(" %[^\n]s", p.address);

    do {
        printf("Enter patient's 10-digit mobile number: ");
        scanf("%s", p.mobile);

        if (!isValidPhoneNumber(p.mobile)) {
            printf("Invalid mobile number. Please enter exactly 10 digits.\n");
        }
    } while (!isValidPhoneNumber(p.mobile));

    // Checkup or Admission selection
    do {
        printf("Enter 1 for regular checkup, 2 for admit: ");
        scanf("%d", &p.checkup);

        if (p.checkup != 1 && p.checkup != 2) {
            printf("Invalid input. Please enter 1 for regular checkup or 2 for admit.\n");
        }
    } while (p.checkup != 1 && p.checkup != 2);

    if (p.checkup == 1) {
        printf("Select doctor type:\n");
        printf("1. General Practitioner\n");
        printf("2. Specialist\n");
        printf("3. Surgeon\n");
        scanf("%d", &p.doctorType);

        int doctorFee = getDoctorFee(p.doctorType);
        totalBill = 500 + doctorFee;
        printf("\nPatient came for a regular checkup.\n");
    } else {
        printf("Enter admit date (dd/mm/yyyy): ");
        scanf("%s", p.admitDate);

        printf("Enter total hours of admission: ");
        scanf("%d", &hoursAdmitted);

        if (hoursAdmitted < 0) {
            hoursAdmitted += 24;
        }

        // Ward Type Selection
        do {
            printf("Enter 1 for General Ward, 2 for ICU: ");
            scanf("%d", &p.wardType);

            if (p.wardType != 1 && p.wardType != 2) {
                printf("Invalid input. Please enter 1 for General Ward or 2 for ICU.\n");
            }
        } while (p.wardType != 1 && p.wardType != 2);

        printf("Select doctor type:\n");
        printf("1. General Practitioner\n");
        printf("2. Specialist\n");
        printf("3. Surgeon\n");
        scanf("%d", &p.doctorType);

        int doctorFee = getDoctorFee(p.doctorType);

        if (p.wardType == 1) {
            totalBill = hoursAdmitted * 200 + doctorFee;
            printf("\nPatient was admitted to the General Ward.\n");
        } else if (p.wardType == 2) {
            totalBill = hoursAdmitted * 500 + doctorFee;
            printf("\nPatient was admitted to the ICU.\n");
        }
    }

    printf("\n----- Patient Bill -----\n");
    printf("Name: %s\n", p.name);
    printf("Age: %d\n", p.age);
    printf("Address: %s\n", p.address);
    printf("Mobile: %s\n", p.mobile);

    if (p.checkup == 1) {
        printf("Purpose: Regular Checkup\n");
        printf("Doctor Type: %d\n", p.doctorType);
        printf("Doctor Fee: ₹%d\n", getDoctorFee(p.doctorType));
    } else {
        printf("Admit Date: %s\n", p.admitDate);
        printf("Total hours of admit: %d\n", hoursAdmitted);
        printf("Ward: %s\n", p.wardType == 1 ? "General Ward" : "ICU");

        printf("Doctor Fee: ₹%d\n", getDoctorFee(p.doctorType));
    }

    printf("Total Bill: ₹%d\n", totalBill);
    printf("--------------------------\n");

    return 0;
}
