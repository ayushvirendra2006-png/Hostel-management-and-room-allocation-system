# Hostel-management-and-room-allocation-system
Hostel management and room allocation system
public Room allocateRoom(Student student) {
    List<Room> eligibleRooms = roomRepository
        .findByHostel_GenderTypeAndOccupiedCountLessThanCapacity(student.getGender());
    
    if (eligibleRooms.isEmpty()) {
        throw new NoRoomAvailableException("No vacant rooms matching criteria");
    }
    
    Room chosen = eligibleRooms.get(0); // or apply preference sorting
    chosen.setOccupiedCount(chosen.getOccupiedCount() + 1);
    roomRepository.save(chosen);
    
    Allocation allocation = new Allocation(student, chosen, LocalDate.now(), null, Status.ACTIVE);
    return allocationRepository.save(allocation).getRoom();
}
