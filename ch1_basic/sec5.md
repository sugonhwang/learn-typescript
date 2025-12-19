> 13~14 강의 과제

<details><summary>과제 13</summary>

#### 문제 1. 회원가입 폼 데이터의 일부만 채워진 상태를 처리하기 위해, 모든 속성이 선택적인 타입을 생성하는 문제입니다.

```
1. 기본 타입 정의:
   - User: 회원 정보 (이름, 이메일, 비밀번호).
   - Partial을 활용: 모든 속성을 선택 속성으로 변경하는 타입을 생성하세요.
2.함수 작성:
   - 함수 이름: updateUserForm.
   - 입력: 기존 사용자 데이터와 업데이트된 폼 데이터.
   - 출력: 업데이트된 사용자 데이터.
```

```typescript
type User = {
  name: string;
  email: string;
  password: string;
};

// 함수 작성
function updateUserForm(user: User, updates: Partial<User>): User {
  return { ...user, ...updates };
}

// 테스트 코드
const currentUser = { name: "Alice", email: "alice@example.com", password: "1234" };
const updatedForm = { email: "new-email@example.com" };

console.log(updateUserForm(currentUser, updatedForm));
// 기대 출력: { name: "Alice", email: "new-email@example.com", password: "1234" }
```

#### 문제 2. 프로필 페이지에 표시할 사용자 정보에서 필요한 속성만 선택하는 문제입니다.

```
1. 기본 타입 정의:
  - UserProfile: 사용자 프로필 정보 (아이디, 이름, 이메일, 주소).
2. Pick을 활용:
  - 프로필 페이지에 필요한 데이터(아이디와 이름)만 추출하는 타입을 정의하세요.
3. 함수 작성:
  - 함수 이름: getProfileSummary.
  - 입력: 전체 사용자 정보.
  - 출력: 필요한 속성만 포함된 객체.
```

```typescript
type UserProfile = {
  id: number;
  name: string;
  email: string;
  address: string;
};

// 함수 작성
function getProfileSummary(user: Pick<UserProfile, "id" | "name">) {
  return { id: user.id, name: user.name };
}

// 테스트 코드
const userProfile = { id: 1, name: "Alice", email: "alice@example.com", address: "123 Main St" };

console.log(getProfileSummary(userProfile));
// 기대 출력: { id: 1, name: "Alice" }
```

#### 문제 3. 데이터베이스 저장 시 민감한 정보를 제외하는 문제입니다.

```
1. 기본 타입 정의
  - User: 사용자 정보 (이름, 이메일, 비밀번호, 역할).

2. Omit을 활용:
  - 민감한 정보를 제외한 타입을 정의하세요. (비밀번호는 제외)

3. 함수 작성:
  - 함수 이름: filterSensitiveInfo.
  - 입력: 전체 사용자 정보.
  - 출력: 민감한 정보가 제외된 객체.
```

```typescript
type User = {
  name: string;
  email: string;
  password: string;
  role: string;
};

// 함수 작성
function filterSensitiveInfo(user: User): Omit<User, "password"> {
  const { password, ...rest } = user;
  return rest;
}

// 테스트 코드
const userInfo = { name: "Alice", email: "alice@example.com", password: "1234", role: "admin" };

console.log(filterSensitiveInfo(userInfo));
// 기대 출력: { name: "Alice", email: "alice@example.com", role: "admin" }
```

#### 문제 4. 팀 관리 시스템을 설계하세요. 각 팀은 여러 멤버로 구성되며, 관리자는 특정 역할에 따라 데이터를 조작할 수 있습니다.

```
1. 기본 타입 정의
```

```typescript
type TeamMember = {
  id: number;
  name: string;
  email: string;
  role: "developer" | "designer" | "manager";
  isActive: boolean;
};
```

```
2. 함수 요구사항:
  - createTeamMember:
    - 새 팀원을 생성하는 함수.
    - Partial을 활용하여 입력 데이터 중 일부만 제공되더라도 기본값으로 초기화합니다.
    - 기본값:
      - role: "developer".
      - isActive: true.
  - filterTeamMembers:
      - 특정 조건에 맞는 팀원만 필터링하는 함수.
      - Pick을 사용해 필터링 기준을 정의합니다.
      - 예: role: "designer" 또는 isActive: false.
  - removeSensitiveInfo:
      - 팀원 목록에서 민감한 정보를 제거하는 함수.
      - Omit을 사용해 이메일 주소를 제외한 데이터를 반환합니다.
```

```typescript
type TeamMember = {
  id: number;
  name: string;
  email: string;
  role: "developer" | "designer" | "manager";
  isActive: boolean;
};

// 1. `createTeamMember` 함수 작성
function createTeamMember(data: Partial<TeamMember> & Pick<TeamMember, "id">): TeamMember {
  return { id: data.id, name: data.name || "", email: data.email || "", role: data.role || "developer", isActive: data.isActive ?? true };
}

// 2. `filterTeamMembers` 함수 작성
function filterTeamMembers(members: TeamMember[], filter: Pick<TeamMember, "role" | "isActive">): TeamMember[] {
  return members.filter((member) => member.role === filter.role && member.isActive === filter.isActive);
}

// 3. `removeSensitiveInfo` 함수 작성
function removeSensitiveInfo(members: TeamMember[]): Omit<TeamMember, "email">[] {
  return members.map(({ email, ...rest }) => rest);
}

// 테스트 코드
const members: TeamMember[] = [
  { id: 1, name: "Alice", email: "alice@example.com", role: "developer", isActive: true },
  { id: 2, name: "Bob", email: "bob@example.com", role: "designer", isActive: false },
  { id: 3, name: "Charlie", email: "charlie@example.com", role: "manager", isActive: true },
];

// 1. 새 팀원 생성
const newMember = createTeamMember({ id: 4, name: "Diana" });
console.log("새 팀원 생성: ", newMember);
// 기대 출력: { id: 4, name: "Diana", email: "", role: "developer", isActive: true }

// 2. 필터링된 팀원 목록
const activeDesigners = filterTeamMembers(members, { role: "designer", isActive: true });
console.log("필터링된 팀원: ", activeDesigners);
// 기대 출력: []

// 3. 민감한 정보 제거
const sanitizedMembers = removeSensitiveInfo(members);
console.log("민감한 정보 제거된 팀원: ", sanitizedMembers);
// 기대 출력: [{ id: 1, name: "Alice", role: "developer", isActive: true }, ...]
```

</details>

<details><summary>과제 14</summary>

#### 1. 전자상거래 플랫폼에서 지역 코드에 따른 배송비를 계산하는 로직을 작성하세요.

```
1. 요구사항
  - 각 지역에 대한 고유 코드와 배송비가 주어집니다.
```

```typescript
type RegionCode = "US" | "EU" | "ASIA" | "AFRICA";
```

```
지역 코드와 배송비를 매핑하는 데이터 구조를 Record 타입으로 정의하세요.
```

```typescript
// 지역 코드 타입 정의
type RegionCode = "US" | "EU" | "ASIA" | "AFRICA";

// 배송비 데이터 정의
const shippingCosts: Record<RegionCode, number> = {
  US: 10,
  EU: 15,
  ASIA: 20,
  AFRICA: 25,
};

// 배송비 계산 함수 작성
function calculateShippingCost(region: RegionCode, costs: Record<RegionCode, number>): number {
  // 에러 처리
  if (!(region in costs)) {
    throw new Error(`해당 지역은 배송이 불가합니다. [지역명: ${region}]`);
  }
  return costs[region];
}

// 테스트 코드
console.log(calculateShippingCost("US", shippingCosts)); // 10
console.log(calculateShippingCost("EU", shippingCosts)); // 15
console.log(calculateShippingCost("ASIA", shippingCosts)); // 20
console.log(calculateShippingCost("AFRICA", shippingCosts)); // 25
try {
  console.log(calculateShippingCost("AUSTRALIA" as RegionCode, shippingCosts)); // 에러 발생
} catch (error) {
  console.error(error.message);
}
```

#### 2. 학생들의 점수를 기록하고 평균 점수를 계산하는 문제입니다.

```
1. 요구사항:
  - 학생 이름(string)과 점수(number)를 매핑하는 데이터를 Record 타입으로 정의하세요.
  - 평균 점수를 계산하는 함수를 작성하세요.

2. 함수 작성:
  - 함수 이름: calculateAverageScore.
  - 입력: 학생 점수 데이터(Record<string, number>).
  - 출력: 모든 학생의 평균 점수(number).
```

```typescript
// 학생 점수 데이터 정의
const scores: Record<string, number> = {
  Alice: 85,
  Bob: 92,
  Charlie: 78,
};

// 평균 점수 계산 함수 작성
function calculateAverageScore(scores: Record<string, number>): number {
  let total = 0;
  for (let sum of Object.values(scores)) {
    total += sum;
  }
  return total / 3;
}

// 테스트 코드
console.log(calculateAverageScore(scores)); // 85
```

### 문제 3. 쇼핑몰에서 각 제품의 이름과 가격을 매핑하고, 특정 제품의 가격을 업데이트하는 기능을 구현하세요.

```
1. 요구사항:
  - 제품 이름(string)과 가격(number)을 매핑하는 데이터를 Record 타입으로 정의하세요.
  - 특정 제품의 가격을 업데이트하는 함수를 작성하세요.
2. 함수 작성:
  - 함수 이름: updateProductPrice.
  - 입력: 제품 가격 데이터(Record<string, number>), 업데이트할 제품 이름과 새로운 가격.
  - 출력: 업데이트된 제품 가격 데이터(Record<string, number>).
```

```typescript
// 제품 가격 데이터 정의
const prices: Record<string, number> = {
  Laptop: 1000,
  Phone: 500,
  Tablet: 300,
};

// 가격 업데이트 함수 작성
function updateProductPrice(prices: Record<string, number>, product: string, newPrice: number): Record<string, number> {
  return { ...prices, [product]: newPrice };
}

// 테스트 코드
console.log(updateProductPrice(prices, "Phone", 550));
// 기대 출력: { Laptop: 1000, Phone: 550, Tablet: 300 }
```

</details>
