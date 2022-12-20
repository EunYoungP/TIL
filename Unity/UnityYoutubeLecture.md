# 2022 Shader & Graphics
# 베르의 유니티 강좌<br><br>


## :blush: 유튜브 '베르의 게임 개발 유튜브' 채널의 '베르의 유니티 강좌(고급)' 내용 정리 문서입니다.<br><br>


## :cat: 강의 유튜브 주소
:point_right: [유튜브 이동](https://youtu.be/O4gWiFFq5cI?list=PLYQHfkihy4Azmb0SzMbd4dm5yAUH0ke4p)<br><br>


## :blue_book: 강의소개
:point_right: 
이 강의는 유니티 엔진 기능과 게임 개발 과정을 알려줍니다.

1. 비히클 시스템 구현하기




## :page_with_curl: 강의 정리
<br>

__Part 1 비히클 시스템 구현하기 __


차량 시스템을 가지고 있는 게임을 개발할 때 필요한 비히클 기능입니다.

차에 탑승하고, 내리고, 운전하는 시스템을 의미합니다.

__요구사항__

(1) 캐릭터를 조작하는 기능이 있을 것

(2) 차량을 조작하는 기능이 있을 것

(3) 캐릭터가 차량에 승하차하는 기능이 있을 것
<br><br>

(1)과 (2)가 공통적으로 요구하는 __조작하는 기능이 있을 것__ 에 대한 내용은 __Controller__ 를 통해 관리해줍니다. Controller로 __CharacterController__ 와 __AIController__ 와 같은 차량과 캐릭터를 조작하는 매체를 컨트롤러로 만들어줍니다.

__Controlable__ 클래스를 만들어 __캐릭터 CharacterControlable__/__차량 VehicleControlable__ 로서의 기능과 조작을 받아 이동/회전 등을 처리하는 기능을 분리시켜줍니다.
e.g. Controllable, Attackable, Damagable, Moveable

__Controlable__ 의 자식클래스에 구현을 강제할 함수의 기능은 이동(Move), 회전(Rotate) 입니다. Character가 추가적으로 할 수 있는 동작은 상호작용(Interact), 점프(Jump) 정도가 있습니다.

상호작용(Interact)은 Character에서는 차량에 탑승하는 동작, Vehicle에서는 차량에서 내리는 동작을 구현합니다.

점프(Jump) 기능은, 차량은 점프 할 수 없기 때문에 애매할 수 있습니다.
이렇게 각 컨트롤러의 동작이 직관적으로 일치하지 않는 경우가 종종 발생합니다.
차량에서 점프 기능을 핸들브레이크 기능으로 대체하여 제작합니다.

```c#
public abstract class Controlable : MonoBehaviour
{
    public Transform cameraArmSocket;
    public Transform cameraArm;

    public abstract void Move();
    public abstract void Rotate();
    public abstract void Jump();
    public abstract void Interact();
} 

// Controlable 자식
public class CharacterControlable : Controlable
{
    public Transform charBody;

    Animator animator;
    Rigidbody rigidbody;

    public float jumpForce = 5f;

    void Start()
    {
        animator = charBody.GetComponent<Animator>();
        rigidbody = GetComponent<Rigidbody>();
    }

    public override void Move(Vector2 input)
    {
        animator.SetFloat("MoveSpeed", input.magnitude);
        Vector3 lookForward = new Vector3(cameraArm.forward.x, 0f, cameraArm.forward.z).normalized;
        Vector3 lookRight = new Vector3(cameraArm.right.x, 0f, cameraArm.right.z).normalized;
        Vector3 moveDir = lookForward * input.y + lookRight * input.x;

        if(input != 0)
        {
            charBody.forward = lookForward;
            transform.position += moveDir * Time.deltaTime * 5f;
        }
    }

    public override void Rotate(Vector2 input)
    {
        if(cameraArm != null)
        {
            Vector3 camAngle = cameraArm.rotation.eulerAngles;
            float x = camAngle.x - input.y;

            if(x < 180f)
            {
                x = Mathf.Clamp(x, -1f, 70f);
            }
            else
            {
                x = Mathf.Clamp(x, 335f, 361f);
            }

            cameraArm.rotation = Quaternion.Euler(x, camAngle.y + input.x, camAngle.z);
        }
    }

    public override void Jump()
    {
        rigidbody.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);

        // 이중 점프, 점프 중 행동 제한 등의 기능 확장 가능
    }

    public override void Interact()
    {
        RaycastHit raycasthit;

        if(Physics.Raycast(new Ray(cameraArm.position + cameraArm.forward, cameraArm.forward), out raycasthit, 1f))
        {
            if(raycasthit.collider.CompareTag("vehicle"))
            {
                var vehicle = hit.collider.gameObject.GetComponent<VehicleControlable>();
            }
        }
    }

    // 승차 기능
    private Ienumerator Ride(VehicleControlable vehicle)
    {
        var controller = FindObjectOfType<Controller>();
        var charBody = GetCompoenet<Ridibody>();
        var charCollider = GetComonent<CapsuleCollider>();
        charBody.isKinematic = true;
        charCollider.isTrigger = true;

        // 캐릭터 운전석 문 옆으로 이동
        while(Vector3.Distance(transform.position, vehicle.ridePosition.position) > 0.1f)
        {
            yield return null;
            transform.position += (vehicle.ridePosition.position - trnasform.position).normalized * Time.deltaTime * 3f;
        }

        // 차문 열리는 애니메이션 실행
        vehicle.InteractDoor(true);
        yield return new WatiSeconds(0.5f);

        // 캐릭터 운전석으로 이동
        while(Vector3.Distacne(trnafsorm.positoin, vehicle.characterSeat.position)>0.1f)
        {
            yield return null;
            transform.position += (vehicle.charcaterSeat.position - transform.position).normalized * Time.deltaTime * 3f;
        }

        // 운전석 이동 애니메이션 추가 부분

        vehicle.driveCharacter = this;
        transform.SetParent(vehicle.characterSeat);

        // 카메라 암의 타겟을 바꿈
        controller.ChangeControlTarget(this, vehicle);
        // 차문 닫기 애니메이션 실행
        vehicle.InteractDoor(false);
    }
}

// Controlable 자식
public class VehicleControlable : Controlable
{
    public Transform characterSeat;
    public Transform ridePosition;

    [SerializedFiled]
    private WheelCollider[] wheelColliders;
    [SerializedFiled]
    private GameObject[] wheelMeshes;

    [SerializedFiled]
    private Animator doorAnimator;

    public CharacterControlable driveCharacter;


    public override void Move(Vector2 input)
    {
        for(int i =0; i < 4; i++)
        {
            QUaternion quat;
            Vector3 position;
            wheelCollidres[i].GetWorldPose(out position, out quat);
            wheelMeshes[i].transform.SetPositionAndRotation(position, quat);
        }

        wheelColliders[0].steerAngle = wheelColliders[1].steerAngle = input.x * 20f;

        for(int i = 0; i < 4; i++)
        {
            wheelColliders[i].motorTorque = input.y * (2000f / 4);

        }
    }

    public override void Rotate(Vector2 input)
    {
        if(cameraArm != null)
        {
            Vector3 camAngle = cameraArm.rotation.eulerAngles;
            float x = camAngle.x - input.y;

            if(x < 180f)
            {
                x = Mathf.Clamp(x, -1f, 70f);
            }
            else
            {
                x = Mathf.Clamp(x, 335f, 361f);
            }

            cameraArm.rotation = Quaternion.Euler(x, camAngle.y + input.x, camAngle.z);
        }
    }

    public override void Jump()
    {
        // 핸드 브레이크
    }

    public override void Interact()
    {
        StartCoroutin(GetOut());
    }

    public void InteractDoor(bool isOpen)
    {
        doorAnimator.SetIntegr("EtatAnim", isOpen ? 1 : 2);
    }

    // 하차 기능
    private Ienumerator GetOut()
    {
        var controller = FindObjectOfType<Controlable>();
        var charBody = GetCompoenet<Ridibody>();
        var charCollider = GetComonent<CapsuleCollider>();

        // 문열기
        InteractDoor(true);
        yield return new WaitSeconds(0.5f);

        // 운전석 -> 차문 옆
        while(Vector3.Distacne(driveCharacter.transform.position, ridePosition.position) > 0.1f)
        {
            yield return null;
            driveCharcater.transform.position += (ridePosition.position - driveCharcater.transform.position).normalized * Time.deltaTime * 3f;
        }

        // 캐리터 충돌 활성화
        charBody.isKinematic = false;
        charCollider.isTrigger = false;

        driveCharcater.transform.SetParent(null);

        // 컨트롤러가 조종할 개체를 차 -> 캐릭터 이동
        controller.ChangeControlTarget(this, driveCharcater);
        driveCharacter = null;

        //문닫기
        InteractDoor(false);
    }
}

public class Controller : Monobehaviour
{
    Controlable controlTarget;

    public void ChangeControlTarget(Controlable origin, Controlable target)
    {
        StartCoroutin(MoveCameraArm(origin, target));
        controlTarget = target;
    }

    public IEnumerator MoveCameraArm(Controlable origin, Controlable target)
    {
        if(origin.cameraArm != null
        {
            var cameraArm = origin.cameraArm;
            Vector3 startPos = cameraArm.position;
            Quaternion startRot = caemraArm.rotation;
            float timer = 0f;
            while(timer <= 1f)
            {
                yield return null;
                timer += Time.deltaTime * 3f;
                camerArm.position = Vector3.Lerp(startPos, target.cameraArmSocket.position, timer);
                camerArm.rotation = Quaternion.SLerp(startRot, target.cameraArmSocekt.rotation, timer);
            }

            camerArm.SetParent(target.camerArmSocket);
            target.cameraArm = cameraArm;
        })
    }
}

// Controller 자식
public class PlayerController : Controller
{
    void Update()
    {
        InputMoveAxis();
        InputRotateAxis();
        InputInteractAction();
        InputJumpAction();
    }

    private void InputMoveAxis()
    {
        controlTarget.Move(new Vector2(Input.GetAxis("Horizontal"), Input.GetAxis("Vertical")));
    }

    private void InputRotateAxis()
    {
        controlTarget.Rotate(new Vector2(Input.GetAxis("Mouse X"), Input.GetAxis("Mouse Y")));
    }

    private void InputInteractAction()
    {
        if(Input.GetKeyDown(KeyCode.E))
        {
            controlTarget.Interact();
        }
    }

    private void InputJumpAction()
    {
        if(Input.GetKeyDown(KeyCode.Space))
        {
            controlTarget.Jump();
        }
    }
}
```

__캐릭터 구현__

AnimatorController 생성 후 float 형태의 MoveSpeed 매개변수를 블렌드 트리에 각각 walk, run, idle로 설정해줍니다.

Controller 게임 오브젝트를 생성하고 PlayerController 스크립트를 추가합니다.

Character 게임 오브젝트에 CharacterControlable 스크립트를 추가하고 프로퍼티들을 할당한 다음, PlayerController의 controlTarget에 할당해줍니다.


__차 구현__

차 오브젝트를 생성하고 vehicle 태그를 설정해줍니다.

차에 rigidbody와 collider를 추가해줍니다.

차의 바퀴 네개에 WheelCollider를 추가해줍니다.

오른쪽 문에 오른쪽 문 애니메이터를 추가해줍니다.

차에 탑승할때 사용할 왼쪽문, 캐릭터가 앉을 시트, 카메라 암의 오브젝트를 생성해줍니다.


__캐릭터 차량 상호작용 구현__

* cameraArm 에서 쏜 Ray에 걸린 콜라이더의 태그가 'vehicle'이라면 차량에 탑승하는 기능을 CharacterControlable::Interact 함수에 구현합니다.

* 캐릭터를 운전석 옆문으로 이동시키고, 문을 열고, 운전석으로 이동시키고, 문을 닫아주는 과정을 구현합니다.
(차량을 피해 운전석 옆문으로 이동하는 네비게이션 기능 생략)

* 


