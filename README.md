#include <iostream>
using namespace std;

const int ROW = 5;
const int COL = 4;
const int TRAIN = 3;

int seats[TRAIN][ROW][COL] = {};
int totalAdults = 0;
int totalTeens = 0;

void showSeats() {
    for (int t = 0; t < TRAIN; t++) {
        cout << " 1 2   3 4 (" << t + 1 << "호차)" << endl;
        cout << "-------------" << endl;
        for (int i = 0; i < ROW; i++) {
            for (int j = 0; j < COL; j++) {
                if (j == 2) cout << "  ";
                cout << seats[t][i][j] << " ";
            }
            cout << endl;
        }
        cout << endl;
    }
}

bool isValid(int t, int r, int c) {
    return t >= 0 && t < TRAIN && r >= 0 && r < ROW && c >= 0 && c < COL;
}

void reserveSeats() {
    showSeats();

    int adult, teen;
    cout << "성인 (25000원): ";
    cin >> adult;
    cout << "청소년 (18000원): ";
    cin >> teen;

    int total = adult + teen;
    int count = 0;

    while (count < total) {
        int t, r, c;
        cout << "몇 호차를 예약 하시겠습니까? ";
        cin >> t;
        cout << "몇 열, 몇 번째 좌석을 예약하시겠습니까? ";
        cin >> r >> c;

        t--; r--; c--;

        if (!isValid(t, r, c)) {
            cout << "예약 가능한 좌석이 아닙니다." << endl;
            continue;
        }

        if (seats[t][r][c] == 1) {
            cout << "이미 예약되었습니다. 다른 자리를 선택하세요." << endl;
            continue;
        }

        seats[t][r][c] = 1;
        count++;
    }

    totalAdults += adult;
    totalTeens += teen;
    cout << "예약이 완료되었습니다." << endl;
}

void changeReservation() {
    int changeCount;
    cout << "바꿀 좌석의 개수를 입력하세요: ";
    cin >> changeCount;

    for (int i = 0; i < changeCount; i++) {
        int t1, r1, c1;
        while (true) {
            cout << "현재 좌석(호차, 열, 번째): ";
            cin >> t1 >> r1 >> c1;
            t1--; r1--; c1--;
            if (!isValid(t1, r1, c1) || seats[t1][r1][c1] != 1) {
                cout << "현재 예약되어 있는 좌석이 아닙니다. 다시 입력하세요." << endl;
            } else break;
        }

        int t2, r2, c2;
        while (true) {
            cout << "바꿀 좌석(호차, 열, 번째): ";
            cin >> t2 >> r2 >> c2;
            t2--; r2--; c2--;
            if (!isValid(t2, r2, c2) || seats[t2][r2][c2] == 1) {
                cout << "예약할 수 없는 자리입니다. 다시 입력하세요." << endl;
            } else {
                seats[t1][r1][c1] = 0;
                seats[t2][r2][c2] = 1;
                cout << "변경되었습니다." << endl;
                break;
            }
        }
    }
}

void showTotalPrice() {
    int total = totalAdults * 25000 + totalTeens * 18000;
    cout << "총 " << total << "원 입니다." << endl;
}

int main() {
    while (true) {
        cout << "**기차 예약 시스템**" << endl;
        cout << "1. 좌석 예약 시스템" << endl;
        cout << "2. 예약 변경" << endl;
        cout << "3. 프로그램 종료" << endl;
        cout << "번호를 입력하세요: ";
        int choice;
        cin >> choice;

        if (choice == 1) {
            reserveSeats();
        } else if (choice == 2) {
            showSeats();
            changeReservation();
        } else if (choice == 3) {
            showSeats();
            showTotalPrice();
            break;
        } else {
            cout << "잘못된 입력입니다." << endl;
        }
    }

    return 0;
}
