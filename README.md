### Controller
</br>
컨트롤러 코드

```java

@RequiredArgsConstructor
@RestController
public class PhoneController {
	
	private final PhoneService phoneService;
	
	@GetMapping("/phone")
	public CMRespDto<?> findAll() {
		return new CMRespDto<>(1, phoneService.전체보기());
	}
	
	@GetMapping("/phone/{id}")
	public CMRespDto<?> findById(@PathVariable Long id) {
		return new CMRespDto<>(1, phoneService.상세보기(id));
	}
	
	@DeleteMapping("/phone/{id}")
	public CMRespDto<?> delete(@PathVariable Long id) {
		phoneService.삭제하기(id);
		return new CMRespDto<>(1, null);
	}
	
	@PutMapping("/phone/{id}")
	public CMRespDto<?> update(@PathVariable Long id, @RequestBody Phone phone) {
		return new CMRespDto<>(1, phoneService.수정하기(id, phone));
	}
	
	@PostMapping("/phone")
	public CMRespDto<?> save(@RequestBody Phone phone) {
		return new CMRespDto<>(1, phoneService.저장하기(phone));
	}
}
```

</br>
</br>

### Service
</br>
서비스 코드

```java

@RequiredArgsConstructor
@Service
public class PhoneService {
	
	private final PhoneRepository phoneRepository;
	
	public List<Phone> 전체보기() {
		return phoneRepository.findAll();
	}
	
	public Phone 상세보기(Long id) {
		return phoneRepository.findById(id).get();
	}
		
	@Transactional
	public void 삭제하기(Long id) {
		phoneRepository.deleteById(id);
	}
	
	@Transactional
	public Phone 수정하기(Long id, Phone phone) {
		// 영속화
		Phone phoneEntity = phoneRepository.findById(id).get();
		
		// 영속화 된 객체를 수정
		phoneEntity.setName(phone.getName());
		phoneEntity.setTel(phone.getTel());
		
		return phoneEntity;
	}	// 서비스 종료시에 영속성 컨텍스트가 변경을 감지해서 flush()로 
	
	@Transactional
	public Phone 저장하기(Phone phone) {
		return phoneRepository.save(phone);
	}
}
```
